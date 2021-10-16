in the main folder build with additional flags
```
go build -o ./build/bin/geth   -gcflags=all='-N -l' -v ./cmd/geth
```


run gdb
```
gdb
```

add file
```
file ./build/bin/geth
```


add break point 
```
break /home/sevki/ethereum/go-ethereum/core/headerchain.go:394
break /home/sevki/ethereum/go-ethereum/core/blockchain.go:2390
```



To list current breakpoints
```
info break
```


run the client
```
run --goerli --syncmode "light" --http --signer=/home/sevki/.clef/clef.ipc
```



To delete a breakpoint
```
del [breakpointnumber]
```
temporarily disable a breakpoint
```
dis [breakpointnumber]
```
To enable a breakpoint
```
en [breakpointnumber]
```
To ignore a breakpoint until it has been crossed x times
```
ignore [breakpointnumber] [x]
```

list variables
```
info locals
bt full
```