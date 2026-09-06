# 神经免疫专病临床科研队列平台：阿里云部署说明

## 文件

部署包：`neuroimmune-cohort-aliyun-v1.0.zip`

压缩包内已经包含完整README、前后端代码、PostgreSQL建库脚本、Docker Compose配置、部署前检查脚本及备份脚本。

## 推荐配置

- 阿里云中国内地ECS：Ubuntu 24.04 LTS，4 vCPU、8 GB内存、100 GB以上加密云盘
- RDS PostgreSQL 16：与ECS同地域、同VPC
- 正式域名：解析至ECS公网IP，并完成ICP备案
- 安全组：公网仅开放80/443；SSH 22仅允许固定管理IP；不开放5432

## 部署命令

解压后进入`neuroimmune-cohort-aliyun/mainland`目录：

```bash
cp .env.aliyun.example .env
chmod 600 .env
```

填写域名、RDS内网地址、数据库账号和初始管理员密码，然后执行：

```bash
chmod +x scripts/*.sh
./scripts/preflight.sh
./scripts/deploy-aliyun.sh
```

健康检查：

```bash
curl https://你的正式域名/api/health
```

正常结果为：

```json
{"ok": true}
```

## 账号

管理员首次登录后必须修改初始密码，然后在“账号管理”中为每名学生建立独立账号和录入人ID。系统会自动记录每条病例的创建人、最后修改人、修改时间和审计日志。

## 既有病例

部署包不包含真实患者资料。现有164条病例需在阿里云环境准备完毕后，通过加密、受审计的迁移流程导入；普通科研CSV无法完整保留姓名、住院号和影像。
