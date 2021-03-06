# Naïve Pub-Sub

A [Publish-Subscribe](https://en.wikipedia.org/wiki/Publish–subscribe_pattern) pattern is a quite fundamential pattern and a great way to understand channels a little better.

Here's how we can implement a simplest message broker:
```go
type Pubsub struct {
	subs map[string][]chan string
	closed bool

	sync.RWMutex
}
```

I like it like this but it might be a bood idea to make an interface like:
```go
type MessageBroker interface {
    // Create a new channel through which
    // a subscriber is able to get messages on desired topic.
    Subscribe(topic string) <-chan string
    // Publish a message to a topic.
    // Return an error if something goes wrong.
    Publish(topic string, msg string) error
    // Close all the sending, prepare the broker
    // to a gracefull shutdown.
    Close()
}
```

Even if it is just a toy and is barely workable, it gives an intuition about PubSub.

All glory goes to Eli Bendersky and his article: https://eli.thegreenplace.net/2020/pubsub-using-channels-in-go
I just walked through with it and added some tests.