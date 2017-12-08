```go
	r := Res{
		Name:"jason",
		Age:22,
	}

	c.JSON(
		200,
		struct {
			responseFormat
			Data Res `json:"data"`
		}{
			responseFormat : responseFormat{
				Msg : "成功",
				Code: 0,
			},
			Data : r,
		},
	)
```
