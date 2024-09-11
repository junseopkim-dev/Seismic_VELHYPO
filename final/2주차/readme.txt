 necis  에서 자료를 다운받는다.

 cd Junseopk/
 cd relocation/
 cd [지진event1(날짜_M규모)]/
# analysis 폴더로 HG, HH, EL 채널 데이터 옮기기
 cd [폴더명(날짜)].a
 mv *HG* ../analysis/
 
 cd [폴더명(날짜)].v
 mv *HH* ../analysis/ 	mv *EL* ../analysis/

 cd analysis/
 mseed2sac *KS*  << seed 압축풀어 sac로 만들기
 gcc script_make_seg.c -o script_make_seg.exe -lm << 컴파일
 ./script_make_seg.exe
 => event origin time : year jday hour min sec
NECIS 진원시(KST) => UTC로 변환후 입력 (-9시간). jday = anaylsis 파일(sac 아닌 파일명)에서 확인
*시각은  analysis의 sac 변환 이전 파일의 시각 입력(sac 아님!)
 =>  event location : latitude longitude
NECIS 참고
 ./work1.sc

*진원에 가까운 순서대로 지진파 관측소 정렬
[lee@localhost analysis]$ gcc hypocentral_distance.c -o hypocentral_distance.exe -lm
[lee@localhost analysis]$ ./hypocentral_distance.exe 
evla evlo depth(NECIS 홈 > 더보기)
37.49 129.13 17


*pstime.dat 작성
velhypo> pstime.dat에 기록
*시각은  analysis의 sac 변환 이전 파일의 시각 입력(sac 아님!)

sac << sac 실행
r [관측소명]*.sac
qdp off
ppk mark


*관측소 골고루 분포해있는 지 확인
map  폴더 이동
event_info.txt >> 지진 위도 경도 입력하기
list_station_used.txt >> pstime.dat에 쓰인 관측소 목록 집어넣기
(list_station_all.txt 양식에 맞춰서 복붙)

쉘 >> 
[lee@localhost map]$ ./map_event.sh

map_event. 파일 열어서 확인 (사용된 지진파는 빨간색으로 강조됨)

*velhypo 실행
cd ../velhypo/
gfortran -o velhypo velhypo-4.for; ./velhypo 

out.dat 확인, 4번째값(오차) 최소인 케이스 찾기(1_1,3_10 가급적 사용x),
오차값 0.1 이하 되도록 보정 수행

최적의 값  out1.dat에 저장

*그림 그리기

1. velhypo 폴더에서 out1.dat 복사
2. velocity_model  폴더 들어가 out1.dat 붙여넣기
3. list_dat 파일에 out1 만 남겨놓기
4. 터미널에 다음 구문 입력
cd ../velocity_model/
./work1.exe 

3. 
out1_vmodel_arr.tx 열고 값 복사

tt_curve_make폴더에 들어가
temp_VELHYPO.tvel 에 붙여넣기
(맨 처음 값 깊이 0으로 고치기)

4. [지진event1(날짜_M규모)]폴더(상위 폴더)의 draw 파일 열기
3~5번째 줄(위도,경도,깊이),  from out1.dat
17~25번째 줄(연,월,일,시,분.초)입력

5. draw 파일 실행(배쉬쉘)
cd..
./draw

------------------------------
.exe
[실행파일이름]

cd : 디랙터리 이동
cp : copy
mv : 파일 - 폴더 잘라서 붙이기
mkdir : 폴더 만들기 
pwd : 현재 디랙토리 위치 확인 
ls : 현재 디랙토리의 폴더 - 파일 목록 확인 
cd - : 이전 디랙토리로 이동



KS.ADO2.ELE.2021.294.21.51.48 << seed 파일 (zip)

[기관명].[관측소명].[채널명].[yy].[jday].[hh].[mm].[sec]


EL
HH
HG

r 데이터 읽기
p 데이터 플롯
qdp off 저해상도 필터 끄기

ppk mark

