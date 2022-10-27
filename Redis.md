# Redis

## Install Redis

```bash
brew install redis
```

## Backup

```bash
go install github.com/yannh/redis-dump-go@latest
# local
redis-dump-go -host localhost -port 6379 > dump.resp
```

## Restore

```bash
redis-cli -h localhost -p 6379 --pipe < dump.resp
```

## Reference

- [Redis Explained - An in-depth tutorial](https://architecturenotes.co/redis/)
