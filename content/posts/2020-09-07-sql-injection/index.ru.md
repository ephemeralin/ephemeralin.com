---
title: "💉 SQL Injection"
date: 2020-09-07T12:54:13+03:00
draft: false
tags: ["java", "security", "best_practice"]
featuredImage: "/posts/images/sql-injection.jpg"
---

Внедрение SQL-кода – это распространённый способ атаки на сервис, работающий с базой данных. Заключается он во внедрении в запрос произвольного кода. SQL injection частенько возможна в тех случаях, когда отсуствует правильная обработка пользовательского ввода.

К примеру, при использовании *Statement* это выглядит так (никогда не повторяйте это на продакшене!):
```java
public StudentsCard badExample(Connection conn, String studentName)
      throws SQLException {
String sql = "SELECT name, age FROM Students WHERE student = '" + studentName +"'";
try (var stmt = conn.createStatement(); var rs = stmt.executeQuery(sql)) {
………}
```

Ну и дальше как в комиксе =)

В данной ситуации следует использовать *PreparedStatement* и bind variables. Попытка заслать вредоносный стейтмент будет корректно обработана и текст будет безопасно экранирован:

```java
public StudentsCard goodExample(Connection conn, String studentName)
      throws SQLException {
String sql = "SELECT name, age FROM Students WHERE student = ?"; try (var ps = conn.prepareStatement(sql)) {
      ps.setString(1, studentName);
      try (var rs = ps.executeQuery()) {
………}
```

Всем **безопасного** кода!
