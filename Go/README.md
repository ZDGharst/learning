# Go

## Notes

Enable dependency tracking for your code via the go.mod file:  
`$ go mod init example.com/hello`

You can import published packages to use them:  
`import "rsc.io/quote"`  
`$ go mod tidy`

This will generate a go.sum for use in authenticating the module. You can redirect where Go finds a module (such as to a local directory) using  
`go mod edit -replace=example.com/greetings=../greetings`  
`go mod tidy`

Functions in Go can return multiple values.  
```golang
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message.
    if name == "" {
        return "", errors.New("empty name")
    }

    // If a name was received, return a value that embeds the name
    // in a greeting message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil
}
```

You can use the predefined Logger settings to set prefix/flags.
```golang
import "log"

. . .

log.SetPrefix("greetings: ")
log.SetFlags(0)
```

The init function will set initial values for variables used in the function.
```golang
func init() {
    rand.Seed(time.Now().UnixNano())
}
```

Golang has a feature called slices.
```golang
myArray := [10]int{0,1,2,3,4,5,6,7,8,9} // firstly we need an array
                                        // to make the slice out of
mySlice1 := myArray[1:5]                // slice from 1st up to 5th
                                        // element, so [1 2 3 4]
mySlice2 := mySlice1[0:2]               // slicing a slice
                                        // [1 2]
```

Working with maps.
```golang
func Hellos(names []string) (map[string]string, error) {
    // A map to associate names with messages.
    messages := make(map[string]string)
    // Loop through the received slice of names, calling
    // the Hello function to get a message for each name.
    for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err
        }
        // In the map, associate the retrieved message with
        // the name.
        messages[name] = message
    }
    return messages, nil
}
```

Working with testing.
```golang
import "testing"

func TestHelloName(t *testing.T) {
    name := "Gladys"
    want := regexp.MustCompile(`\b`+name+`\b`)
    msg, err := Hello("Gladys")
    if !want.MatchString(msg) || err != nil {
        t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
    }
}
```

Tests start with Test, `go test` will run all files that end with _test.go. Optionally use -v for a verbose test.

## Pointers

Go has pointers. A pointer holds the memory address of a value.

The type *T is a pointer to a T value. Its zero value is nil.

```golang
var p *int
```

The & operator generates a pointer to its operand.

```golang
i := 42
p = &i
```

The * operator denotes the pointer's underlying value.

```golang
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

This is known as "dereferencing" or "indirecting".

Unlike C, Go has no pointer arithmetic. 