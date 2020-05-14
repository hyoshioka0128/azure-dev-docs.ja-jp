---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: d4b9becdce2b78e928b97b7d980024eac5871df2
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369825"
---
`DemoApplication` クラスの横に新しい `Todo` Java クラスを作成します。

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

このクラスは、先ほど作成した `todo` テーブルにマップされるドメイン モデルです。

このクラスを管理するには、リポジトリが必要です。 同じパッケージ内に新しい `TodoRepository` インターフェイスを定義します。

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

このリポジトリは、Spring Data R2DBC によって管理されるリアクティブ リポジトリです。

データを格納および取得できるコントローラーを作成して、アプリケーションを完成させます。 同じパッケージに `TodoController` クラスを実装し、次のコードを追加します。

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最後に、アプリケーションを停止して再起動します。

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>アプリケーションをテストする

アプリケーションをテストするには、cURL を使用します。

まず、データベースに新しい "todo" 項目を作成します。

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

次のコマンドを実行すると、作成した項目が返されます。

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

次に、新しい cURL 要求を使用してデータを取得します。

```bash
curl http://127.0.0.1:8080
```

このコマンドを実行すると、作成した項目を含む "todo" 項目の一覧が返されます。

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```
