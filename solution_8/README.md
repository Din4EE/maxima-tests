### Ответ

```text
undefined behavior
```

Результат выполнения программы всегда будет разный.

Запустим с флагом --race:

```text
go run --race test7.go  
```

И увидим:

```text
==================
WARNING: DATA RACE
```

Как и в предыдущем задании переменная `num` не защищена от конкуретного доступа, а `i` захватывается замыканием по указателю на область памяти.
Также программа может завершиться быстрее, чем все горутины отработают.

Чтобы решить эту задачу и избавиться от состояния гонки без оверхеда, можно воспользоваться примитивами синхронизации `atomic` и `WaitGroup`

```go
var num int32

func main() {
    // Какой будет результат выполнения приложения
    wg := &sync.WaitGroup{}
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        counter := i
        go func() {
            defer wg.Done()
            atomic.StoreInt32(&num, int32(counter))
        }()
    }
    wg.Wait()
    fmt.Printf("NUM is %d\n", atomic.LoadInt32(&num))
}
```

Мы все еще будем получать разные значения, так как планировщик запускает горутины в случайном порядке, но зато мы обеспечили атомарный доступ к переменной `num`, а также избавились от Race Condition.