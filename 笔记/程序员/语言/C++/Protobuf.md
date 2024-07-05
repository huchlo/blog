Protocol Buffer 跨平台的序列化数据结构的协议，比json更小更快，以二进制数据方式保存

>见解：适用于微服务间远程调用（RPC），快速传输对象；各不同语言编写的系统间通信；客户端与服务端保持长连接时传输数据（保密性更强）

官网： https://protobuf.dev/

protoc --version

在 js/ts 中使用，需node环境，安装下面protobufjs和protobufjs-cli，就可以用pbjs和pbts指令了

```sh
#https://www.npmjs.com/package/protobufjs
npm i protobufjs -g
#Command line interface (CLI) for [protobuf.js
#https://www.npmjs.com/package/protobufjs-cli
npm i protobufjs-cli -g

# proto to js
pbjs -t static-module -w commonjs -o CSProto.js CSProto.proto
# js to ts
pbts -o CSProto.d.ts CSProto.js

```

描述文件*.proto