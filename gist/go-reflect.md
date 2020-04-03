---
layout: gist
title: Go (Reflect)
---

# Go (Reflect)

### Print function of packages
```go
subPackage := "app/controller"

set := token.NewFileSet()
packs, err := parser.ParseDir(set, subPackage, nil, 0)
if err != nil {
	fmt.Println("Failed to parse package:", err)
	os.Exit(1)
}

// funcs := []*ast.FuncDecl{}
for _, pack := range packs {
	for _, f := range pack.Files {
		for _, d := range f.Decls {
			if fn, isFn := d.(*ast.FuncDecl); isFn {
				// funcs = append(funcs, fn)
				fmt.Println(fn.Name)

			}
		}
	}
}
```

### Assign pointer to other with reflection

```go
n := 42
p := &n

x := new(int)
// set the value to *x, but x must be initialized
reflect.ValueOf(x).Elem().Set(reflect.ValueOf(p).Elem())
fmt.Println("*x:", *x)

var y *int
// to set the value of y directly, requires y be addressable
reflect.ValueOf(&y).Elem().Set(reflect.ValueOf(p))
fmt.Println("*y:", *y)
```

### Get Package Name

```go
// If the type was predeclared (string, error) or not defined (*T, struct{},
// []int, or A where A is an alias for a non-defined type), the package path
// will be the empty string.
func packageName(v interface{}) string {
	val := reflect.ValueOf(v)
	if val.Kind() == reflect.Ptr {
		return val.Elem().Type().PkgPath()
	}
	return val.Type().PkgPath()
}
```