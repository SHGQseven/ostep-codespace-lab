## Q1
- Prediction / 预测:
总时间：10 tick，CPU利用率100%，IO利用率0%
- Reasoning / 理由:两个进程全部是CPU指令，不会产生IO事件，不会触发IO切换。操作系统等待PID0全部执行完毕之后，才调度PID1。全程CPU没有空闲。
- Verified result / 验证结果:总时间：35
CPU 利用率：CPU 一共工作 8 个单位，8/35 ≈ %22.86%
IO 利用率：IO 一共工作 8 个单位，8/35 ≈ %22.86%
- Analysis / 分析:

## Q2
- Prediction / 预测:总时间：11 tick，CPU利用率 6/11 ≈54.55%，IO利用率5/11≈45.45%
- Reasoning / 理由:PID0连续执行4条CPU指令，执行完成后切换到PID1。PID1执行IO指令后进入阻塞状态，需要等待5个tick完成IO，IO结束后再消耗1tick完成io_done。整个过程一共11个tick，IO等待期间CPU没有其他就绪进程，CPU空闲。
- Verified result / 验证结果:总时间：**35**
CPU 总工作：8，CPU 利用率 = 8/35 ≈ **22.86%**
IO 总工作：8，IO 利用率 = 8/35 ≈ **22.86%**
- Analysis / 分析:

## Q3
- Prediction / 预测:总时间：11 tick，CPU利用率9/11≈81.82%，IO利用率5/11≈45.45%
- Reasoning / 理由:PID0首先执行IO指令，随即进入阻塞。在IO等待的5个tick期间，操作系统切换到PID1，PID1连续执行4条CPU指令。IO完成后，再用1tick执行io_done。总时长11tick，IO等待期间CPU被PID1利用，减少了CPU空闲。
- Verified result / 验证结果:总时间：**35**
CPU 总工作：8，CPU 利用率 = 8/35 ≈ **22.86%**
IO 总工作：8，IO 利用率 = 8/35 ≈ **22.86%**
- Analysis / 分析:

## Q4
- Prediction / 预测:总时间：11 tick，CPU利用率5/11≈45.45%，IO利用率5/11≈45.45%
- Reasoning / 理由:调度策略为SWITCH_ON_END，仅在进程结束时切换。PID0发起IO后阻塞，但不会切换到PID1，CPU空闲等待5个tick直到IO完成。PID0执行io_done结束后，才切换到PID1执行4条CPU指令。总时长11tick，IO等待阶段CPU空闲。
- Verified result / 验证结果:总时间：**56**
CPU 总工作：8，CPU 利用率 = 8/56 ≈ **14.29%**
IO 总工作：8，IO 利用率 = 8/56 ≈ **14.29%**
- Analysis / 分析:

## Q5
- Prediction / 预测:总时间：11 tick，CPU利用率9/11≈81.82%，IO利用率5/11≈45.45%
- Reasoning / 理由:调度策略SWITCH_ON_IO，进程发起IO就马上切换。PID0执行IO后阻塞，立刻切换到PID1运行4条CPU指令，IO等待期间CPU被充分利用。IO完成后执行io_done，总时长11tick。
- Verified result / 验证结果:总时间：**8**
CPU 总工作：2，CPU 利用率 = 2/8 = **25%**
IO 总工作：1，IO 利用率 = 1/8 = **12.5%**
- Analysis / 分析:

## Q6
- Prediction / 预测:总时间：21 tick，CPU利用率15/21≈71.43%，IO利用率5/21≈23.81%
- Reasoning / 理由:PID0先执行3次IO，每次发起IO都会切换到其他就绪进程运行。IO完成后不会立即唤醒PID0，需要等待调度。3次IO每次占用5tick等待，其余3个进程各5条CPU指令交替执行。总tick数21，IO等待时CPU运行其他进程，提升CPU利用率。
- Verified result / 验证结果:总时间：**8**
CPU 总工作：2，CPU 利用率 = 2/8 = **25%**
IO 总工作：1，IO 利用率 = 1/8 = **12.5%**
- Analysis / 分析:

## Q7
- Prediction / 预测:总时间：21 tick，CPU利用率15/21≈71.43%，IO利用率5/21≈23.81%
- Reasoning / 理由:SWITCH_ON_IO，发起IO立刻切换。IO_RUN_IMMEDIATE策略，IO一旦完成，立即运行该进程。PID0的3次IO完成后会抢占CPU，改变进程执行顺序。总tick总数依旧21，CPU和IO利用率与Q6一致，但进程执行顺序不同。
- Verified result / 验证结果:总时间：**14**
CPU 总工作：2，CPU 利用率 = 2/14 ≈ **14.29%**
IO 总工作：1，IO 利用率 = 1/14 ≈ **7.14%**
- Analysis / 分析:

## Q8
- Prediction / 预测:种子1（默认 SWITCH_ON_IO, IO_RUN_LATER）：总时间3 tick，CPU利用率100%，IO利用率0%
种子1（IO_RUN_IMMEDIATE）：总时间3 tick，CPU利用率100%，IO利用率0%
种子1（SWITCH_ON_END）：总时间3 tick，CPU利用率100%，IO利用率0%

种子2（默认 SWITCH_ON_IO, IO_RUN_LATER）：总时间24 tick，CPU利用率6/24=25%，IO利用率20/24≈83.33%
种子2（IO_RUN_IMMEDIATE）：总时间24 tick，CPU利用率6/24=25%，IO利用率20/24≈83.33%
种子2（SWITCH_ON_END）：总时间26 tick，CPU利用率6/26≈23.08%，IO利用率20/26≈76.92%

种子3（默认 SWITCH_ON_IO, IO_RUN_LATER）：总时间19 tick，CPU利用率6/19≈31.58%，IO利用率15/19≈78.95%
种子3（IO_RUN_IMMEDIATE）：总时间19 tick，CPU利用率6/19≈31.58%，IO利用率15/19≈78.95%
种子3（SWITCH_ON_END）：总时间21 tick，CPU利用率6/21≈28.57%，IO利用率15/21≈71.43%
- Reasoning / 理由:参数 `-l 3:50,3:50` 代表两个进程，每个进程最多3条指令，每条指令有50%概率是CPU指令或IO指令，不同随机种子生成不同指令序列。
1. SWITCH_ON_IO：进程发起IO时，立刻切换到就绪进程；IO等待期间CPU可以运行其他进程，减少CPU空闲。
2. IO_RUN_LATER：IO完成后，进程进入就绪队列，等待轮到自己才运行。
3. IO_RUN_IMMEDIATE：IO一旦完成，立刻抢占CPU运行该进程；本实验中两个进程IO不重叠，因此总时间和IO_RUN_LATER一致。
4. SWITCH_ON_END：**只有进程全部执行完毕才切换**，进程发起IO阻塞时不会切换，CPU全程空闲等待IO，所以总运行时间最长，CPU利用率最低。
种子1没有任何IO指令，所有策略下运行时间完全相同，CPU利用率100%。IO指令越多，IO等待造成的空闲时间越多，整体总时间变长。
- Verified result / 验证结果:总时间：**39**
CPU 总工作：3+5+5 = 13
CPU 利用率 = 13/39 ≈ **33.33%**
IO 总工作：5+5 =10
IO 利用率 =10/39 ≈ **25.64%**
- Analysis / 分析:

