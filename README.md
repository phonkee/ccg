# ccg
ccg generates codes from existing template package.

features
* no DSL for template
* no directive comments

# install
```
go get github.com/reusee/ccg/cmd/ccg
```

# command example

### sorter
```
ccg -from github.com/reusee/ccg/sorter -params T=[]byte -renames Sorter=BytesSorter
```
output
```go
type BytesSorter struct {
  Slice [][]byte
  Cmp   func(a, b []byte) bool
}

func (s BytesSorter) Len() int {
  return len(s.Slice)
}

func (s BytesSorter) Less(i, j int) bool {
  return s.Cmp(s.Slice[i], s.Slice[j])
}

func (s BytesSorter) Swap(i, j int) {
  s.Slice[i], s.Slice[j] = s.Slice[j], s.Slice[i]
}
```

### set
```
ccg -from github.com/reusee/ccg/set -params T=string -renames Set=StrSet,New=NewStrSet -package foobar
```
output
```go
package foobar

type StrSet map[string]struct{}

func NewStrSet() StrSet {
  return StrSet(make(map[string]struct{}))
}

func (s StrSet) Add(t string) {
  s[t] = struct{}{}
}

func (s StrSet) Del(t string) {
  delete(s, t)
}

func (s StrSet) In(t string) (ok bool) {
  _, ok = s[t]
  return
}
```

# writing template package
Any package with placeholder type defined can be used as template.

Check following packages for example
* https://github.com/reusee/ccg/tree/master/sorter
* https://github.com/reusee/ccg/tree/master/set
* https://github.com/reusee/ccg/tree/master/infchan

# others

### command is too verbose
write a wrapper (e.g. https://github.com/reusee/ccg/tree/master/cmd/myccg) or define shell aliases

### insert outputs to vim
:read !ccg [args...]
