문제 설명<br>
추석 트래픽<br>
이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다.<br> 
장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인<br> 
9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다.<br> 
초당 최대 처리량은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.<br>

입력 형식<br>
solution 함수에 전달되는 lines 배열은 N(1 ≦ N ≦ 2,000)개의 로그 문자열로 되어 있으며,<br> 
각 로그 문자열마다 요청에 대한 응답완료시간 S와 처리시간 T가 공백으로 구분되어 있다.<br>
응답완료시간 S는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 2016-09-15 hh:mm:ss.sss 형식으로 되어 있다.<br>
처리시간 T는 0.1s, 0.312s, 2s 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 s로 끝난다.<br>
예를 들어, 로그 문자열 2016-09-15 03:10:33.020 0.011s은 "2016년 9월 15일 오전 3시 10분 33.010초"부터<br> 
"2016년 9월 15일 오전 3시 10분 33.020초"까지 "0.011초" 동안 처리된 요청을 의미한다. (처리시간은 시작시간과 끝시간을 포함)<br>
서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 0.001 ≦ T ≦ 3.000이다.<br>
lines 배열은 응답완료시간 S를 기준으로 오름차순 정렬되어 있다.<br>

출력 형식<br>
solution 함수에서는 로그 데이터 lines 배열에 대해 초당 최대 처리량을 리턴한다.<br>
<br><br><br>


    def get_time(time):
        hour = int(time[:2])*3600
        minute = int(time[3:5])*60
        second = int(time[6:8])
        milli = int(time[9:])
        return (hour+minute+second) * 1000 + milli

    def start_time(time, process):
        prtime = int(float(process[:-1])*1000)
        return get_time(time)-prtime + 1

    def solution(lines):
        answer = 0
        end=[]
        start=[]

        for l in lines:
            time = l.split(' ')
            end.append(get_time(time[1]))
            start.append(start_time(time[1],time[2]))

        for i in range(len(lines)):
            cnt = 0
            nowend = end[i]

            for y in range(i, len(lines)):
                if nowend > start[y]-1000:
                    cnt += 1
            answer = max(answer, cnt)

        return answer
