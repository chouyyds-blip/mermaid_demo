flowchart TD
    %% 定义样式
    classDef op fill:#e1f0fa,stroke:#6fa8dc,stroke-width:1px
    classDef judge fill:#fff2cc,stroke:#d6b656,stroke-width:1px
    classDef update fill:#d5e8d4,stroke:#82b366,stroke-width:1px
    classDef keep fill:#f0f0f0,stroke:#999999,stroke-width:1px
    classDef title fill:#1f4e79,stroke:#1f4e79,color:white,font-weight:bold

    %% 第一部分：初始化与迭代停止判断
    subgraph 第一部分：初始化与迭代停止判断
        direction LR
        start[开始]:::op
        input[输入初始解S0]:::op
        init[初始化参数：<br>温度T、权重ω、<br>当前解=最优解=S0、<br>迭代次数iter=0]:::op
        judge1{是否满足最大<br>迭代次数<br>(iter ≥ Nmax)?}:::judge
        end[输出全局最优解<br>结束]:::update

        start --> input --> init --> judge1
        judge1 -- 是 --> end
    end

    %% 第二部分：迭代循环核心
    subgraph 第二部分：迭代循环核心
        direction LR
        step1[1. 轮盘赌选择<br>破坏算子<br>(5种)]:::op
        step2[2. 执行破坏算子<br>移除部分客户]:::op
        step3[3. 选择并执行<br>修复算子<br>(3种)]:::op
        step4[4. 局部搜索优化<br>2-opt + 路径交换<br>(每次5次)]:::op
        step5[5. 计算新解成本<br>Z(S_new)]:::op
        judge2{6. 是否优于<br>当前最优解?}:::judge
        update1[更新最优解<br>Sbest = Snew]:::update
        keep1[保持最优解<br>Sbest 不变]:::keep
        judge3{7. 是否接受<br>当前解?<br>(模拟退火准则)}:::judge
        update2[接受新解<br>Scurrent = Snew]:::update
        keep2[拒绝新解<br>保持当前解不变<br>Scurrent 不变]:::keep
        step8[8. 更新算子权重<br>自适应更新]:::op
        step9[9. 温度下降<br>T = T × 0.995]:::op
        step10[10. 迭代次数加1<br>iter = iter + 1]:::op

        %% 核心流程连线
        judge1 -- 否 --> step1
        step1 --> step2 --> step3 --> step4 --> step5 --> judge2
        judge2 -- 是 --> update1 --> judge3
        judge2 -- 否 --> keep1 --> judge3
        judge3 -- 是 --> update2 --> step8
        judge3 -- 否 --> keep2 --> step8
        step8 --> step9 --> step10 --> step1
    end

    %% 图例
    subgraph 图例
        direction LR
        legend1[算法操作]:::op
        legend2[判断条件]:::judge
        legend3[更新操作]:::update
        legend4[保持不变]:::keep
    end
