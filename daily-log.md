# Daily Log

> 20.05.03
> 
> ### bash 문법
> 조건문
> ```bash
> if [ 조건 ]
> then
> 	# true
> else
> 	# false
> fi
> ```
> [if문](https://ansan-survivor.tistory.com/539)  
> 
> diff 명령어  
> 파일을 비교하는 명령어
> ```bash
> diff [옵션] file1 file2
> ```
> ```bash
> diff file1 file2
> 
> # 1c2   
> # < aaaa
> # ---
> # > bbbb
> ```
> ```bash
> diff -u file1 file2
> 
> # --- file1       2022-05-03 21:01:24.000000000 +0900
> # +++ file2       2022-05-03 21:01:30.000000000 +0900
> # @@ -1 +1 @@
> # -aaaa
> # +bbbb
> ```
> 파일 정보가 두 줄로 출력되는데 file1은 `-` 로 표시되고, file2는 `+` 로 표시된다는 내용이다. `@@ -1 +1 @@`는 `-` (file1)의 1 line과 `+` (file2)의 1 line의 내용이라는 뜻이다.
>   
> [diff 사용법](https://muyu.tistory.com/entry/diff-%EC%82%AC%EC%9A%A9%EB%B2%95)

> 20.05.02  
> ### bash 문법  
> 파일의 내용을 한 줄씩 가져오기
> ```bash
> path="/path/to/filename"
> while IFS= read -r line
> do
> 	command1 on $line
> 	command2 on $line
> 	..
> 	....
> 	commandN
> done < "$path"
> ```    
> [while..do..done bash loop](https://bash.cyberciti.biz/guide/While_loop)  
> 
> 함수 사용하기
> ```bash
> # function의 파라미터는 $n
> function test {
> 	echo $1
> 	echo $2
> }
> test "hello" "world"
> ```
> [function bash](https://www.fun25.co.kr/blog/bash-function-example/?category=005)


> 20.05.01  
> 깃북과 깃허브를 동기화하여 TIL 페이지 생성  
>  - 동기화가 완료되면 깃허브에 마크다운 파일이 생성되고, 마크다운 파일을 수정하여 깃북을 업데이트할 수 있다.




