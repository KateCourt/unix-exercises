# Episode 7 Finding Things

## 7.1 Using `grep`

Which command would result in the following output:

~~~
and the presence of absence:
~~~

1. `grep "of" haiku.txt`
2. `grep -E "of" haiku.txt`
3. `grep -w "of" haiku.txt`
4. `grep -i "of" haiku.txt`

<details>
<summary>Solution</summary>
The correct answer is 3, because the `-w` option looks only for whole-word matches. The other options will also match ‘of’ when part of another word.
</details>
