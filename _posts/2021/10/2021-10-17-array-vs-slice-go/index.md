---
layout: post
title:  "array vs slice in Golang"
date:   2021-10-17 09:12:00 +0800
categories: golang
---

# array

The traits of array
1. Fixed-length (zero or more), once declared, we can't change the size
2. Sequence of elements (with particular type)
3. Seldom used, slice is more common.
4. We can compare it using `==`.
5. Passed by value, means array value will be copied to the corresponding parameter variable. 
   1. Use pointer if we want to pass array by reference

One practical example is SHA digest data structure: [sha256.go]
```
package sha256
// ... omitted for brevity

// Sum256 returns the SHA256 checksum of the data.
func Sum256(data []byte) [Size]byte {
	var d digest
	d.Reset()
	d.Write(data)
	return d.checkSum()
}
```

# slice

The traits of slice
1. Sequence with variable-length, with the same type
2. It has 3 components
   1. pointer (to underlying backend array)
   2. length
   3. capacity
3. We can't compare it using `==`. To perform slice comparison, we need to write our own function, except for [bytes.Equal]
4. Use `append` function to add elements to slice.

Read [Golang book] chapter 4.1 and 4.2 for more details :)

[sha256.go]: https://cs.opensource.google/go/go/+/refs/tags/go1.17.2:src/crypto/sha256/sha256.go;l=254
[bytes.equal]: https://pkg.go.dev/bytes#Equal
