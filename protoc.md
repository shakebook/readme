## protoc教程 ##
问题表现

编译protobuf的 .pb.go文件时报错，如 undefined: grpc.SupportPackageIsVersion6 或 undefined: grpc.ClientConnInterface

和这个贴子的表现一样，https://github.com/grpc/grpc-go/issues/3347

解决办法

方法1：升级grpc到1.27以上即可，但是如果升级后出现了其他报错，如 undefined: resolver.BuildOption 或 undefined: resolver.ResolveNowOption，又必须降低grpc版本到1.26或以下时，请使用方法2

方法2：降级protoc-gen-go的版本

注意：使用命令 go get -u github.com/golang/protobuf/protoc-gen-go 的效果是安装最新版的protoc-gen-go

降低protoc-gen-go的具体办法，在终端运行如下命令，这里降低到版本 v1.2.0

GIT_TAG=”v1.2.0″
go get -d -u github.com/golang/protobuf/protoc-gen-go
git -C “$(go env GOPATH)”/src/github.com/golang/protobuf checkout $GIT_TAG
go install github.com/golang/protobuf/protoc-gen-go


编译输出go文件:
protoc --go_out=plugins=grpc:.. service/account/proto/user.proto 