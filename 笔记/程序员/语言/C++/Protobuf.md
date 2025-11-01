Protocol Buffer 跨平台的序列化数据结构的协议，比json更小更快，以二进制数据方式保存

>见解：适用于微服务间（可不同语言）远程调用（RPC），快速传输对象；客户端与服务端保持长连接时传输数据（byte[]）

官网： https://protobuf.dev/

protoc --version

## 在C++中使用
```sh
#编译*.proto，输出 *.pb.cc、*.pb.h文件
#编译CSProto.proto，输出到./当前目录
protoc --cpp_out=./ CSProto.proto
#编译当前目录的所有.proto文件，并输出到当前目录
protoc -I=./ --cpp_out=./ *.proto
```

## 在Java中使用
```sh

#编译*.proto，输出 {package}/*.java文件
#编译CSProto.proto，输出到./当前目录
protoc --java_out=./ GameProto.proto
#编译当前目录的所有.proto文件，并输出到当前目录
protoc -I=./ --java_out=./ *.proto

#java socket
#DataInputStream in = new DataInputStream(socket.getInputStream());  
#in.readAllBytes();  
#DataOutputStream out = new DataOutputStream(socket.getOutputStream());  
#out.write(xxx);
```

## 在 js/ts 中使用
需node环境，安装下面protobufjs和protobufjs-cli，就可以用pbjs和pbts指令了

```sh
#https://www.npmjs.com/package/protobufjs
npm i protobufjs -g
#Command line interface (CLI) for [protobuf.js
#https://www.npmjs.com/package/protobufjs-cli
npm i protobufjs-cli -g

# proto to js
pbjs -t static-module -w commonjs -o CSProto.js CSProto.proto
pbjs -t static-module -w commonjs -o TestProto.js TestProto.proto
pbjs -t static-module -w commonjs -o GameProto.js GameProto.proto
# js to ts
pbts -o CSProto.d.ts CSProto.js
pbts -o TestProto.d.ts TestProto.js
pbts -o GameProto.d.ts GameProto.js
#得到CSProto.d.ts CSProto.js文件
```

描述文件*.proto