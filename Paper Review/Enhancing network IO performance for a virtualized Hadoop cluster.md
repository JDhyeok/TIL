# Enhancing network I/O performance for a virtualized Hadoop cluster (작성중)

http://csl.skku.edu/uploads/Publications/ccpe17.pdf

## 0. Summary

MapReduce 프로그래밍 모델은 주로 사용되는 클라우드 컴퓨팅 프레임워크중 하나인 Hadoop을 이용한 빅데이터 처리 프로세스에서 사용된다. 클라우드 컴퓨팅 사용의 증가함에 따라 가상화된 Hadoop 클러스터를 사용하는 것이 cost를 감소시키고 효율성을 증가시키는 좋은 접근 방법이다.

해당 논문에서, 가상 네트워크에서의 performance를 측정하고 가상화된 클러스터에서 작동하는 Hadoop workloads의 네트워크 performance의 영향에 대해서 분석했다.

public/private 클라우드 제공자에게 가상 Hadoop 클러스터를 위해서 **가상화된 네트워크 I/O 아키텍처**가 가장 최적화된 구조임을 제안한다.

또한 제안된 컴퓨팅 환경에서의 Hadoop 프레임워크 rack awareness 기능을 더 나은 방법으로 응용하는 것을 보여준다.

이 네으워크 아키텍처는 브릿지 네트워크 아키텍처보다 약 4.1배 performance 향상됐다.

Keyword : Hadoop, performance, virtualization

## 1. INTRODUCTION

