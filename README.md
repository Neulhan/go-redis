# Go Redis 초간단 튜토리얼 해보기

[go-redis/redis](https://github.com/go-redis/redis)

## 설치하기

```bash
go mod init github.com/my/repo
go get github.com/go-redis/redis/v7
```

## import

```go
import "github.com/go-redis/redis"
```

## ping

```go
func ping() {
	redisClient := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379", // redis 서버 주소 (redis 의 디폴트 포트 6379 로컬호스트)
		Password: "",               // redis 비밀번호
		DB:       0,                // redis DB 번호 선택
	})
	// Ping 함수 (redis 서버는 사용자와 탁구를 칠 수 있습니다 하하)
	pong, err := redisClient.Ping().Result()
	fmt.Println(pong, err)
}
```

## redis set

```go
// redis 에 cache set
err := redisClient.Set("key", "value", 0).Err()
if err != nil {
	panic(err)
}
```

## redis get

```go
// redis 에서 cache get
val, err := redisClient.Get("key").Result()

if err != nil {
	panic(err)
}

fmt.Println("key", val)
```

## redis 예외처리

```go
// redis client 에 key 값이 존재하지 않는 경우 예외처리
val2, err := redisClient.Get("key2").Result()
if err == redis.Nil {
	// key 에 대응되는 값이 존재하지 않을 경우
	fmt.Println("key2 does not exist")
} else if err != nil {
	// 에러 발생
	panic(err)
} else {
	// key 에 대응되는 값을 찾았을 경우
	fmt.Println("key2", val2)
}
```

## 후기

main 함수에서 redis client 를 선언해주고, 필요할 때 가져다 쓰면 될 듯.
이것을 go echo 에 보다 효과적으로 붙이는 방법은 좀 더 찾아봐야 할 듯하다.  

곧 Go Echo 에 적용하는 방법도 간단한 튜토리얼로 올리고 링크를 아래에 첨부하도록 하겠습니다.