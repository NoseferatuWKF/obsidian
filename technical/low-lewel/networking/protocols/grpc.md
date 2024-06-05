protobuf
```proto
syntax = "proto3";

service Foo {
	rpc ReqRes (Empty) returns (Something) {}
	rpc ServerStream (Something) returns (stream Something) {}
	rpc TwoWayStream (stream Something) returns (stream Something) {}
}

message Empty {}

message Something {
	int32 int = 1;
	string data = 2;
}

message ListSomething {
	repeated Something data = 1;
}
```