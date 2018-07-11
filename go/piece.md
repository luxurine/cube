

## struct 到 map[string]interface{} 的转化，并以 tag 里的名字作为key

```go
package main

import (
	"fmt"
	"reflect"
)

type VehicleInfo struct {
	// ID         bson.ObjectId `bson:"_id,omitempty"`
	VehicleId string `bson:"编号"`
	Date      string `bson:"日期"`
	Type      string `bson:"类型"`
	Brand     string `bson:"型号"`
	Color     string `bson:"颜色"`
}

func main() {
	vinter := map[string]interface{}{}
	vinfo := VehicleInfo{
		VehicleId: "123456",
		Date:      "20140101",
		Type:      "Truck",
		Brand:     "Ford",
		Color:     "White",
	}
	vt := reflect.TypeOf(vinfo)
	vv := reflect.ValueOf(vinfo)
	for i := 0; i < vt.NumField(); i++ {
        f := vt.Field(i)
        chKey := f.Tag.Get("bson")
		fmt.Printf("%q => %q\n ", chKey, vv.FieldByName(f.Name).String())
		vinter[chKey] = vv.FieldByName(f.Name)
	}
	fmt.Println(vinter)
}
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"reflect"
)

type Response struct {
	Name string
	Age  int
}

type User struct {
	Action   string
	Username string
	Password string
	DataSet  []Response
}

func Struct2Map(obj interface{}) map[string]interface{} {
	t := reflect.TypeOf(obj)
	v := reflect.ValueOf(obj)

	var data = make(map[string]interface{})
	for i := 0; i < t.NumField(); i++ {
		data[t.Field(i).Name] = v.Field(i).Interface()
	}
	return data
}

func main() {
	xx := Response{"wwb", 10}
	user := User{"1331", "zhangsan", "pwd", []Response{xx}}
	data := Struct2Map(user)
	OutResponse(data)
}

func OutResponse(v interface{}) (err error) {
	temp := v.(map[string]interface{})

	actionName := temp["Username"].(string)
	temp["Action"] = actionName + "Response"

	b, err := json.Marshal(v)
	if err != nil {
		fmt.Printf("[OutResponse]Fatal json.Marshal error:%+v", err)
		return err
	}
	fmt.Println(string(b))
	return nil
}
}
```

## Struct Conventions
[github opensource tool: fatih/structs](https://github.com/fatih/structs)

// Create a new struct type:
s := structs.New(server)
> m := s.Map()              // Get a map[string]interface{}
v := s.Values()           // Get a []interface{}
f := s.Fields()           // Get a []*Field
n := s.Names()            // Get a []string
f := s.Field(name)        // Get a *Field based on the given field name
f, ok := s.FieldOk(name)  // Get a *Field based on the given field name
n := s.Name()             // Get the struct name
h := s.HasZero()          // Check if any field is initialized
z := s.IsZero()           // Check if all fields are initialized

jess
sda
asdsa
