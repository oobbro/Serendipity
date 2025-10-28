<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>温馨提示弹窗</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        /* 弹窗核心样式 - 响应式大小 */
        .tip-box {
            position: fixed;
            padding: 12px 20px;
            border-radius: 12px; /* 圆角更柔和 */
            font-family: "微软雅黑", "PingFang SC", sans-serif;
            font-size: clamp(14px, 3vw, 18px); /* 自适应字体大小 */
            color: #2d3748;
            z-index: 9999; /* 确保置顶 */
            box-shadow: 0 4px 16px rgba(0,0,0,0.15); /* 增强阴影层次感 */
            opacity: 0;
            transform: translate(-50%, -50%) scale(0.8) rotate(-5deg);
            animation: float 4s ease-in-out forwards; /* 绑定动画 */
            max-width: 80vw; /* 手机端不超屏幕宽度 */
            min-width: 120px;
        }
        /* 新增：飘落+旋转+淡入淡出动画 */
        @keyframes float {
            0% {
                opacity: 0;
                transform: translate(-50%, -50%) scale(0.8) rotate(-5deg);
            }
            20% {
                opacity: 1;
                transform: translate(-50%, -50%) scale(1) rotate(2deg);
            }
            80% {
                opacity: 1;
                transform: translate(-50%, -50% + 15px) scale(1) rotate(-1deg); /* 轻微飘落 */
            }
            100% {
                opacity: 0;
                transform: translate(-50%, -50% + 30px) scale(0.9) rotate(3deg);
            }
        }
        /* 背景色优化：更柔和的马卡龙色 */
        .bg-pink { background: #fef7fb; color: #e53e3e; }
        .bg-blue { background: #e8f4f8; color: #4299e1; }
        .bg-green { background: #f0f8fb; color: #48bb78; }
        .bg-purple { background: #faf0f5; color: #9f7aea; }
        .bg-yellow { background: #fefaf0; color: #ed8936; }
        .bg-coral { background: #fef5f5; color: #e53e3e; }
        .bg-aqua { background: #eaf6fa; color: #38b2ac; }
    </style>
</head>
<body>
    <script>
        // 【可自定义配置项】- 方便修改
        const CONFIG = {
            totalTips: 88, // 弹窗总数（比之前多，更热闹）
            showDuration: 4000, // 单个弹窗显示时长（4秒，可改）
            createInterval: 20, // 弹窗创建间隔（20毫秒，避免卡顿）
            maxRotate: 8, // 最大旋转角度（增加灵动性）
        };

        // 提示文字列表（新增几条更有趣的）
        const tips = [
            '多喝水哦~', '保持微笑呀', '每天都要元气满满',
            '记得吃水果', '保持好心情', '好好爱自己', '死恋爱脑',
            '好好学习', '加油备考', '梦想成真', '期待下一次见面',
            '金榜题名', '顺顺利利', '早点休息', '愿所有烦恼都消失',
            '别熬夜', '今天过得开心嘛', '天冷了，多穿衣服',
            '摸鱼适度呀', '好运正在加载', '你超棒的！', '记得拉伸～'
        ];

        // 绑定背景色类名（对应CSS）
        const bgClasses = [
            'bg-pink', 'bg-blue', 'bg-green', 'bg-purple',
            'bg-yellow', 'bg-coral', 'bg-aqua'
        ];

        // 生成随机位置（优化：基于屏幕中心偏移，更均匀）
        function getRandomPos() {
            const screenWidth = window.innerWidth;
            const screenHeight = window.innerHeight;
            // 弹窗位置范围限制在屏幕内，避免超出
            const x = Math.random() * (screenWidth - 100) + 50;
            const y = Math.random() * (screenHeight - 80) + 40;
            return { x, y };
        }

        // 创建单个弹窗（优化逻辑，更简洁）
        function createTipBox() {
            const tipBox = document.createElement('div');
            const randomTip = tips[Math.floor(Math.random() * tips.length)];
            const randomBg = bgClasses[Math.floor(Math.random() * bgClasses.length)];
            const { x, y } = getRandomPos();
            const randomRotate = (Math.random() - 0.5) * 2 * CONFIG.maxRotate; // 随机旋转角度

            // 赋值样式和内容
            tipBox.className = `tip-box ${randomBg}`;
            tipBox.textContent = randomTip;
            tipBox.style.left = `${x}px`;
            tipBox.style.top = `${y}px`;
            tipBox.style.transform = `translate(-50%, -50%) rotate(${randomRotate}deg)`;
            tipBox.style.animationDuration = `${CONFIG.showDuration / 1000}s`; // 绑定显示时长

            // 添加到页面 + 自动移除
            document.body.appendChild(tipBox);
            setTimeout(() => {
                document.body.removeChild(tipBox);
            }, CONFIG.showDuration);
        }

        // 批量创建弹窗（优化：用setTimeout循环，避免阻塞）
        for (let i = 0; i < CONFIG.totalTips; i++) {
            setTimeout(createTipBox, i * CONFIG.createInterval);
        }
    </script>
</body>
</html>
