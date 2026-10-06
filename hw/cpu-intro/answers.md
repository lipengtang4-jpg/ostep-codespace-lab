# OSTEP Ch.4 Homework Answers

All predictions were written before using the answer trace option. CPU utilization = CPU-busy ticks / total ticks.

## Q1 — process list 5:100,5:100
- Prediction / 预测: Total 10 ticks; CPU 10/10 = 100%. PID 0 runs first for five ticks, then PID 1; neither process issues I/O.
- Reasoning / 理由 (state table):
|Time|PID 0|PID 1|CPU|
|1|RUN|READY|1|
|2|RUN|READY|1|
|3|RUN|READY|1|
|4|RUN|READY|1|
|5|RUN|READY|1|
|6|DONE|RUN|1|
|7|DONE|RUN|1|
|8|DONE|RUN|1|
|9|DONE|RUN|1|
|10|DONE|RUN|1|
- Verified result / 验证结果:
- Analysis / 分析:

## Q2 — process list 4:100,1:0
- Prediction / 预测: Total 11 ticks; CPU 6/11 = 54.55%. PID 1 waits until PID 0 finishes; I/O costs one start tick, five blocked ticks, and one completion tick.
- Reasoning / 理由 (state table):
|Time|PID 0|PID 1|CPU|IOs|
|1–4|RUN|READY|1 each| |
|5|DONE|RUN:io|1|1|
|6–10|DONE|BLOCKED|idle|1 each|
|11*|DONE|RUN:io_done|1| |
- Verified result / 验证结果:
- Analysis / 分析:

## Q3 — process list 1:0,4:100
- Prediction / 预测: Total 7 ticks; CPU 6/7 = 85.71%.
- Reasoning / 理由 (state table):
|Time|PID 0|PID 1|CPU|IOs|
|1|RUN:io|READY|1|1|
|2–5|BLOCKED|RUN|1 each|1 each|
|6|BLOCKED|DONE|idle|1|
|7*|RUN:io_done|DONE|1| |
PID 1 keeps the CPU busy during the first four wait ticks; one blocked tick remains after it finishes.
- Verified result / 验证结果:
- Analysis / 分析:

## Q4 — process list 1:0,4:100, switch on end
- Prediction / 预测: Total 11 ticks; CPU 6/11 = 54.55%.
- Reasoning / 理由 (state table):
|Time|PID 0|PID 1|CPU|IOs|
|1|RUN:io|READY|1|1|
|2–6|BLOCKED|READY|idle|1 each|
|7*|RUN:io_done|READY|1| |
|8–11|DONE|RUN|1 each| |
No switch on I/O leaves the CPU idle for five ticks although PID 1 is READY.
- Verified result / 验证结果:
- Analysis / 分析:

## Q5 — process list 1:0,4:100, switch on I/O
- Prediction / 预测: Total 7 ticks; CPU 6/7 = 85.71%, same as Q3.
- Reasoning / 理由: Tick 1 PID 0 starts I/O; ticks 2–5 PID 1 runs; tick 6 CPU is idle with PID 0 blocked; tick 7 PID 0 handles completion. Six CPU ticks total.
- Verified result / 验证结果:
- Analysis / 分析:

## Q6 — three I/Os, switch on I/O, run later
- Prediction / 预测: Total 31 ticks; CPU 21/31 = 67.74%; I/O busy 15/31 = 48.39%.
- Reasoning / 理由: Hand timeline: tick 1 P0 starts I/O; ticks 2–6 P1 CPU; 7–11 P2 CPU (P0 I/O finishes at 7); 12–16 P3 CPU; 17 P0 I/O completion; 18 P0 starts its second I/O; 19–23 CPU idle; 24 P0 completion; 25 P0 starts third I/O; 26–30 CPU idle; 31 P0 completion. P1–P3 contribute 15 CPU ticks and P0 contributes six I/O start/completion ticks.
- Verified result / 验证结果:
- Analysis / 分析:

## Q7 — three I/Os, switch on I/O, run immediately
- Prediction / 预测: Total 21 ticks; CPU 21/21 = 100%; I/O busy 15/21 = 71.43%.
- Reasoning / 理由: Hand timeline: tick 1 P0 starts I/O; 2–6 P1 CPU; 7 P0 completion; 8 P0 starts I/O; 9–13 P2 CPU; 14 P0 completion; 15 P0 starts I/O; 16–20 P3 CPU; 21 P0 completion. The CPU-only processes cover all three waits.
- Verified result / 验证结果:
- Analysis / 分析:

## Q8 — seeded workloads, three settings

Instruction lists read without answer traces:
- Seed 1: P0 CPU, I/O, completion, I/O, completion; P1 three CPU instructions.
- Seed 2: P0 I/O, completion, I/O, completion, CPU; P1 CPU, I/O, completion, I/O, completion.
- Seed 3: P0 CPU, I/O, completion, CPU; P1 I/O, completion, I/O, completion, CPU.

- Prediction / 预测:
  - Seed 1: default 15 ticks, CPU 8/15 = 53.33%; immediate 15 ticks, 53.33%; switch-on-end 18 ticks, 8/18 = 44.44%.
  - Seed 2: default 16 ticks, 10/16 = 62.50%; immediate 16 ticks, 62.50%; switch-on-end 23 ticks, 10/23 = 43.48%.
  - Seed 3: default 18 ticks, 9/18 = 50.00%; immediate 17 ticks, 9/17 = 52.94%; switch-on-end 24 ticks, 9/24 = 37.50%.
- Reasoning / 理由: CPU work is fixed by each instruction list. Switching on I/O lets the other process run during waits. Immediate return can preempt the other process when I/O finishes; I expect seed 3 to finish one tick sooner. Switch-on-end may leave CPU idle while the current process waits, so I expect longer runs and lower utilization.
- Hand timeline / 手算时间线: Seed 1 default/immediate: tick 1 P0 CPU; 2 P0 I/O; 3–5 P1 CPU; 6–7 idle; 8 P0 completion; 9 P0 second I/O; 10–14 idle; 15 P0 completion. Under switch-on-end, P1 runs at ticks 16–18. Seed 2 default/immediate finish at tick 16; switch-on-end at tick 23. Seed 3 default at tick 18; immediate at tick 17; switch-on-end at tick 24.
- Verified result / 验证结果:
- Analysis / 分析:
