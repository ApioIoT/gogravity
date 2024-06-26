## GoGravity

An SDK for work with [Apio Gravity](https://gitlab.com/apio-private/apio-gravity) in Golang.

#### Install
```bash
go get github.com/ApioIoT/gogravity
```

#### Base setup
```golang
gravity := gogravity.New("http://localhost:7000")

// Test connection
if err := gravity.Ping(context.Background()); err != nil {
  log.Fatal(err)
}

// Setup a topic (set true on the second parameter create a topic if not exists)
topic, err := gravity.Topic("project.resource.action", true)
if err != nil {
  log.Fatal(err)
}
```

#### Enqueue a job
```golang
type Payload struct {
  Message string `json:"message"`
}

if err := topic.Enqueue(Payload{ Message: "ciao" }); err != nil {
  log.Fatal(err)
}
```

#### Dequeue a job
```golang
job, err := topic.Dequeue()
if err != nil {
  t.Fatal(err)
}
```

#### Listen for a job
```golang
jobs, cancel, err := topic.Listen("* * * * * *", "Europe/Rome")
if err != nil {
  log.Fatal(err)
}
defer func() {
  if err := cancel(); err != nil {
    log.Fatal(err)
  }
}()

for job := range jobs {
  // ...
}
```

#### Complete a job
```golang
if err := gravity.Complete(job, nil); err != nil {
  log.Fatal(err)
} 
```

#### Fail a job
```golang
if err := gravity.Fail(job, nil); err != nil {
  log.Fatal(err)
}
```

#### Return a job
```golang
if err := gravity.Return(job); err != nil {
  log.Fatal(err)
} 
```

#### Add a schedule
```golang
if err := topic.AddSchedule("*/5 * * * * *", "Europe/Rome", true, false, 0); err != nil {
  t.Fatal(err)
}
```
