# Linux 프로세스 관리

ps : 현재 터미널 프로세스
ps aux : 전체 프로세스
top : 실시간 확인

문제가 있는 프로세스를 kill(종료) 시키기 위함.

ps aux | grep sleep : sleep 찾기 (sleep은 예시 프로세스임)
kill 숫자 : 해당 PID 종료

예시: 실습용
sleep 300 & (프로세스를 300초동안 대기시킴)
ps aux | grep sleep
kill 3354 3354는 PID