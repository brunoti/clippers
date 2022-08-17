# Bull board for NestJS - Today I Learned
```
const app = await NestFactory.create(AppModule, {
  // ...
})

const serverAdapter = new ExpressAdapter()
serverAdapter.setBasePath('/bull-board')

const aQueue = app.get<Queue>(
  `BullQueue_<queue_name>`
)

createBullBoard({
  queues: [
    new BullAdapter(aQueue),
  ],
  serverAdapter,
})

app.use(
  '/bull-board',
  expressBasicAuth({
    users: {
      user: 'password',
    },
    challenge: true,
  }),
  serverAdapter.getRouter()
)
```

TypeScript

Bull board will be protected with basic HTTP authentication.