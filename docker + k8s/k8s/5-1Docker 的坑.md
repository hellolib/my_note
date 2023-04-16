## 时区问题

> docker 默认UTC时间

- 解决办法:

  ```sh
  # 在docker 容器内部 修改etc 的localtime时间
  cp /usr/share/zoneinfo/PRC /etc/localtime
  ```

  