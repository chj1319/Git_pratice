# 배달 로봇 사양 


| 장치 | 갱신 주기 | 1회 데이터 (가정) | 데이터율 | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| **바퀴 엔코더** | 1 kHz (1 ms) | 2륜 × 4 B 카운터 = 8 B | 8 KB/s | 모터 제어 루프의 피드백 |
| **IMU** | 200 Hz (5 ms) | 가속도 3축 + 각속도 3축 × 4 B + 타임스탬프 8 B = 32 B | 6.4 KB/s | 자세·오도메트리 융합 |
| **2D 라이다** | 10 Hz (100 ms) | 360 점 × (거리 4 B + 세기 4 B) = 2,880 B | 28.8 KB/s ≈ 0.23 Mbps | 장애물 감지 |
| **RGB 카메라** | 30 fps (33 ms) | 1920 × 1080 × 3 B = 6,220,800 B ≈ 6.22 MB | 186.6 MB/s ≈ 1,493 Mbps | 보행자 인식 (원시 영상 기준) |
| **LTE 모듈** | — | — | 업링크 이론 최대 50 Mbps (Cat.4), 실측 5~20 Mbps | 왕복 지연(RTT) 30~100 ms, 터널·지하에서 끊김 |

> **주행 및 제동 조건**  
> 배달 로봇의 주행 속도는 보도 주행 규정에 맞춰 $v = 1.5\text{ m/s}$, 감속도 $a = 2\text{ m/s}^2$ 로 가정
> 이 값으로 제동 거리는 $v^2 / 2a = 0.56\text{ m}$ 이고, **100 ms 반응 지연마다 0.15 m 씩 더 진행**


## 2. 원격 접속(SSH)과 센서 장치 경로 고정

1. localhost로 접속

```console
pa33@pa33-Legion-Pro-5-16IAX10:~$ ssh pa33@localhost
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 6.8.0-138-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

Expanded Security Maintenance for Applications is not enabled.

189 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

143 additional security updates can be applied with ESM Apps.
Learn more about enabling ESM Apps service at https://ubuntu.com/esm

New release '24.04.4 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Last login: Tue Aug 25 14:14:47 2026 from 127.0.0.1
pa33@pa33-Legion-Pro-5-16IAX10:~$ who
pa33     tty2         2026-08-25 00:09 (tty2)
pa33     pts/3        2026-08-25 14:14 (127.0.0.1)
pa33     pts/4        2026-08-25 14:15 (127.0.0.1)
pa33@pa33-Legion-Pro-5-16IAX10:~$ echo $SSH_CONNECTION
127.0.0.1 44232 127.0.0.1 22

```