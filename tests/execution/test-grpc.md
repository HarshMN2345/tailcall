# test-grpc

###### check identity

#### server:

```graphql
schema @server(port: 8000) @upstream(baseURL: "http://localhost:50051", batch: {delay: 10, headers: [], maxSize: 1000}) @link(id: "news", src: "../../src/grpc/tests/news.proto", type: Protobuf) {
  query: Query
}

input NewsInput {
  body: String
  id: Int
  postImage: String
  title: String
}

type News {
  body: String
  id: Int
  postImage: String
  title: String
}

type NewsData {
  news: [News]!
}

type Query {
  news: NewsData! @grpc(method: "news.NewsService.GetAllNews")
  newsById(news: NewsInput!): News! @grpc(body: "{{args.news}}", method: "news.NewsService.GetNews")
  newsByIdBatch(news: NewsInput!): News! @grpc(body: "{{args.news}}", groupBy: ["news", "id"], method: "news.NewsService.GetMultipleNews")
}
```