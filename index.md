# RRRuboCop

---

# だれこれ

---

<img style="width: 20%; border-radius: 50%" alt="ほにゃー" src="pocke.svg">

## Masataka Kuwabara (a.k.a. pocke)

- Actcat Inc.
- PORT Inc.

---

# RuboCop とは

---


Ruby のLintツール

- 問題がありそうなRubyコードを指摘してくれる


```
db/migrate/20160906073718_create_users_github_repositories.rb:17:41:
		C: Rails/NotNullColumn: Do not add a NOT NULL column without a default value
		add_column :users, :email, :string, null: false
											^^^^^^^^^^^

1 file inspected, 1 offense detected
```

```
test.rb:1:1:
		W: The use of eval is a serious security risk.
		eval xxx
		^^^^

1 file inspected, 1 offense detected
```

---


# Problem

---


- RuboCop は遅い
  - シングルスレッドで動く
  - RubyはGILがあるためマルチスレッドにしてもIOが絡まないと意味がない
    - Global Interpretor Lock (またはGVL Giant VM Lockとも)


キャッシュが効いてる状態でもxxx秒

---

#  Solution

---


マルチプロセスでRuboCopを動かすぞ!!!!1


---

# RRRuboCop

---

[pocke/rrrubocop](https://github.com/pocke/rrrubocop)

- マルチプロセスでRuboCopを動かすコマンド
  - プロセスを分ければGILかんけーない
- 名前はcookpad/rrrspecをinspired

---

# Current State

---

- 一応4C8Tのマシンで2.6倍程度の高速化
  - Intel HTTやオーバーヘッドを考えると、3倍の高速化を目標にしたい
- しかしまだWorker -> Master へのデータの受け渡しが実装できてない……
  - `Marshal.dump`にハマった

---

# まとめ

---

- 今日は動くところまで持っていけなかった…
- RuboCopを速く実行したい方、期待してて下さい!
