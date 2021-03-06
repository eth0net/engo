# Embed Demo

## What does it do?

It demonstrates how one can bundle assets into their binary to reduce distribution complexity using the go embed package

## What are important aspects of the code?

These lines are key in this demo:

- `//go:embed *.png *.tmx` to embed the files
- `var fs embed.FS` to store the embedded filesystem
- `data, err := assets.ReadFile(file)` to retrieve a bundled asset
- `err = engo.Files.LoadReaderData(file, bytes.NewReader(data))` to load it into engo.Files

## Load each file in a list

```go
func (s *GameScene) Preload() {
	files := []string{
		"tilemap.tmx",
		"sprites.png,
	}
	for _, file := range files {
		data, err := assets.ReadFile(file)
		if err != nil {
			log.Fatalf("Unable to locate asset with URL: %v\n", file)
		}
		err = engo.Files.LoadReaderData(file, bytes.NewReader(data))
		if err != nil {
			log.Fatalf("Unable to load asset with URL: %v\n", file)
		}
	}
}
```

## Setup a bundler go file to use go generate

assets/assets.go

```go
//+build demo

package assets

import "embed"

//go:embed *.png *.tmx
var fs embed.FS

// ReadFile wrapper for embedded assets filesystem.
func ReadFile(name string) ([]byte, error) {
	return fs.ReadFile(name)
}
```
