# Daily Log

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




