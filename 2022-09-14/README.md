RDS Aurora fail over 테스트
============================
RDS를 클러스터로 운영시, fail over 등으로 writer가 reader로 교체되면
기존 writer에 접속된 connection은 reader 속성으로 변경되어 insert/update/delete와 같은 쿼리가 실행되지 않음.<br>(ERROR 1290 (HY000): The MySQL server is running with the –read-only option so it cannot execute this statement)

해당 커넥션은 계속 살아있게 되므로, 오류가 무한히 지속되는데 golang 기준으로 db.SetConnMaxLifetime(time.Second * 10)
을 설정하여 커넥션의 최대 사용시간을 지정해주면 writer로 재연결되므로 무한히 오류가 나는 것을 방지할 수 있음.

재연결로인한 오버헤드가 클것이라 예상되었으나, 거의 미미하여 짧게 사용해도 무방할 정도 이다.

인스턴스 재부팅 시간은 10초 내외이며, writer/reader 전환 역시 10초 내외로 테스트 되었다.
