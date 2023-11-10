# Dockerfile

## 1. Dockerfile ARG 穿参找不到

- 错误使用; MAIN_PATH 将找不到

  ```dockerfile
  ARG MAIN_PATH
  
  FROM xxx AS builder
  
  RUN make build MAIN_PATH=${MAIN_PATH}
  ```

- 正确使用

  ```dockerfile
  FROM xxx AS builder
  
  ARG MAIN_PATH
  
  RUN make build MAIN_PATH=${MAIN_PATH}
  ```

  