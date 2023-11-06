### Ответ
```text
0 1 2
0 0 0
```

В этом блоке констант мы объявляем их построчно и таким образом увеличиваем `iota` от 0 до 2.

```go
const (
	A = iota //0
	B = iota //1
	C = iota //2
)
```
А во втором блоке мы объявляем D E F в одной строке. 
`iota` также начинается с 0 так как это новый блок констант, но из-за того что все переменные объявлены в одной строке, `iota` не увеличивается и остается равна 0.
В общем случае это связано со спецификой определения переменных.

```go
const (
	D, E, F = iota, iota, iota //0
)
```