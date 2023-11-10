# go mod

## 1. go mod tidy报错： ... but go 1.16 would select v1.13.0

- 修复

  ```go
  go mod tidy -go=1.16 && go mod tidy -go=1.17  // 这会选择依赖版本为Go 1.16，然后选择Go 1.17 
  // 或者
  go mod tidy -compat=1.17  //这只是删除Go 1.16校验和（因此提示“不需要Go 1.16的再现性”）。
  ```

  