<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>王晗艺术签名生成器</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 30px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 30px;
            margin-top: 20px;
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 2.2rem;
        }
        
        .description {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            line-height: 1.6;
        }
        
        .signature-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 30px 0;
        }
        
        .canvas-container {
            width: 100%;
            height: 300px;
            background: #f9f9f9;
            border: 2px dashed #ddd;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
            overflow: hidden;
        }
        
        #signatureCanvas {
            max-width: 100%;
            max-height: 100%;
        }
        
        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            margin-bottom: 20px;
        }
        
        .style-btn {
            padding: 10px 15px;
            background: #4a6ee0;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 0.9rem;
        }
        
        .style-btn:hover {
            background: #3a5bc7;
            transform: translateY(-2px);
        }
        
        .style-btn.active {
            background: #2a4bb3;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .color-picker {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .color-option {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
            transition: transform 0.2s;
        }
        
        .color-option:hover {
            transform: scale(1.2);
        }
        
        .color-option.active {
            border-color: #333;
            transform: scale(1.2);
        }
        
        .action-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 20px;
        }
        
        .action-btn {
            padding: 12px 25px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .save-btn {
            background: #4CAF50;
            color: white;
        }
        
        .save-btn:hover {
            background: #45a049;
            transform: translateY(-2px);
        }
        
        .reset-btn {
            background: #f44336;
            color: white;
        }
        
        .reset-btn:hover {
            background: #da190b;
            transform: translateY(-2px);
        }
        
        .instructions {
            background: #f0f7ff;
            padding: 15px;
            border-radius: 8px;
            margin-top: 30px;
            border-left: 4px solid #4a6ee0;
        }
        
        .instructions h3 {
            color: #333;
            margin-bottom: 10px;
        }
        
        .instructions p {
            color: #555;
            line-height: 1.5;
        }
        
        .github-guide {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin-top: 30px;
            border: 1px solid #e1e1e1;
        }
        
        .github-guide h2 {
            color: #333;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .github-steps {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .step {
            display: flex;
            align-items: flex-start;
            gap: 15px;
        }
        
        .step-number {
            background: #4a6ee0;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            font-weight: bold;
        }
        
        .step-content {
            flex: 1;
        }
        
        .step-content h4 {
            margin-bottom: 5px;
            color: #444;
        }
        
        .step-content p {
            color: #666;
            line-height: 1.5;
        }
        
        footer {
            margin-top: 30px;
            text-align: center;
            color: #777;
            font-size: 0.9rem;
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .controls {
                flex-direction: column;
                align-items: center;
            }
            
            .action-buttons {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <h1>王晗艺术签名生成器</h1>
    <p class="description">选择您喜欢的签名风格和颜色，生成专属艺术签名</p>
    
    <div class="container">
        <div class="signature-container">
            <div class="canvas-container">
                <canvas id="signatureCanvas" width="600" height="250"></canvas>
            </div>
            
            <div class="controls">
                <button class="style-btn active" data-style="elegant">优雅风格</button>
                <button class="style-btn" data-style="bold">豪放风格</button>
                <button class="style-btn" data-style="modern">现代风格</button>
                <button class="style-btn" data-style="classic">古典风格</button>
            </div>
            
            <div class="color-picker">
                <div class="color-option active" style="background-color: #000000;" data-color="#000000"></div>
                <div class="color-option" style="background-color: #4a6ee0;" data-color="#4a6ee0"></div>
                <div class="color-option" style="background-color: #d32f2f;" data-color="#d32f2f"></div>
                <div class="color-option" style="background-color: #388e3c;" data-color="#388e3c"></div>
                <div class="color-option" style="background-color: #7b1fa2;" data-color="#7b1fa2"></div>
                <div class="color-option" style="background-color: #f57c00;" data-color="#f57c00"></div>
            </div>
            
            <div class="action-buttons">
                <button class="action-btn save-btn" id="saveBtn">
                    <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                        <path d="M11 2H5a1 1 0 0 0-1 1v10a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V3a1 1 0 0 0-1-1zM5 1a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V3a2 2 0 0 0-2-2H5z"/>
                        <path d="M8.5 6.5a.5.5 0 0 0-1 0v3.793L6.354 9.146a.5.5 0 1 0-.708.708l2 2a.5.5 0 0 0 .708 0l2-2a.5.5 0 0 0-.708-.708L8.5 10.293V6.5z"/>
                    </svg>
                    保存签名
                </button>
                <button class="action-btn reset-btn" id="resetBtn">
                    <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                        <path d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2v1z"/>
                        <path d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466z"/>
                    </svg>
                    重置签名
                </button>
            </div>
        </div>
        
        <div class="instructions">
            <h3>使用说明</h3>
            <p>1. 选择不同的风格按钮可以切换签名样式</p>
            <p>2. 点击颜色圆点可以更改签名颜色</p>
            <p>3. 点击"保存签名"按钮可以将签名保存为PNG图片</p>
            <p>4. 点击"重置签名"按钮可以恢复到默认设置</p>
        </div>
        
        <div class="github-guide">
            <h2>GitHub Pages 部署指南</h2>
            <div class="github-steps">
                <div class="step">
                    <div class="step-number">1</div>
                    <div class="step-content">
                        <h4>创建GitHub账户</h4>
                        <p>如果您还没有GitHub账户，请访问 <a href="https://github.com" target="_blank">github.com</a> 注册一个新账户。</p>
                    </div>
                </div>
                <div class="step">
                    <div class="step-number">2</div>
                    <div class="step-content">
                        <h4>创建新仓库</h4>
                        <p>登录GitHub后，点击右上角的"+"号，选择"New repository"。为仓库命名（例如"wanghan-signature"），选择公开(Public)，并勾选"Add a README file"。</p>
                    </div>
                </div>
                <div class="step">
                    <div class="step-number">3</div>
                    <div class="step-content">
                        <h4>上传代码文件</h4>
                        <p>在仓库页面，点击"Add file" → "Upload files"，将本HTML文件拖拽到上传区域，然后点击"Commit changes"。</p>
                    </div>
                </div>
                <div class="step">
                    <div class="step-number">4</div>
                    <div class="step-content">
                        <h4>启用GitHub Pages</h4>
                        <p>进入仓库的"Settings"选项卡，在左侧菜单中找到"Pages"。在"Source"部分，选择"Deploy from a branch"，然后选择"main"分支和"/ (root)"文件夹，点击"Save"。</p>
                    </div>
                </div>
                <div class="step">
                    <div class="step-number">5</div>
                    <div class="step-content">
                        <h4>访问您的网站</h4>
                        <p>几分钟后，您的网站将可以通过类似 <code>https://[您的用户名].github.io/[仓库名]</code> 的URL访问。</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <footer>
        <p>王晗艺术签名生成器 &copy; 2023 | 使用HTML5 Canvas技术 | 部署指南：GitHub Pages</p>
    </footer>

    <script>
        // 获取Canvas元素和上下文
        const canvas = document.getElementById('signatureCanvas');
        const ctx = canvas.getContext('2d');
        
        // 当前样式和颜色
        let currentStyle = 'elegant';
        let currentColor = '#000000';
        
        // 初始化签名
        function drawSignature() {
            // 清除画布
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // 设置字体和颜色
            ctx.fillStyle = currentColor;
            ctx.strokeStyle = currentColor;
            ctx.lineWidth = 2;
            
            // 根据样式绘制签名
            switch(currentStyle) {
                case 'elegant':
                    drawElegantStyle();
                    break;
                case 'bold':
                    drawBoldStyle();
                    break;
                case 'modern':
                    drawModernStyle();
                    break;
                case 'classic':
                    drawClassicStyle();
                    break;
            }
        }
        
        // 优雅风格
        function drawElegantStyle() {
            ctx.font = 'bold 80px "楷体", "STKaiti", "华文楷体", serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            
            // 绘制"王"字
            ctx.save();
            ctx.translate(canvas.width * 0.35, canvas.height * 0.5);
            ctx.rotate(-0.05);
            ctx.fillText('王', 0, 0);
            ctx.restore();
            
            // 绘制"晗"字
            ctx.save();
            ctx.translate(canvas.width * 0.65, canvas.height * 0.5);
            ctx.rotate(0.05);
            ctx.fillText('晗', 0, 0);
            ctx.restore();
            
            // 添加装饰线条
            ctx.beginPath();
            ctx.moveTo(canvas.width * 0.25, canvas.height * 0.7);
            ctx.bezierCurveTo(
                canvas.width * 0.35, canvas.height * 0.75,
                canvas.width * 0.65, canvas.height * 0.75,
                canvas.width * 0.75, canvas.height * 0.7
            );
            ctx.stroke();
        }
        
        // 豪放风格
        function drawBoldStyle() {
            ctx.font = 'bold 100px "黑体", "SimHei", "Microsoft YaHei", sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            
            // 绘制"王"字
            ctx.save();
            ctx.translate(canvas.width * 0.35, canvas.height * 0.5);
            ctx.scale(1.1, 1);
            ctx.fillText('王', 0, 0);
            ctx.restore();
            
            // 绘制"晗"字
            ctx.save();
            ctx.translate(canvas.width * 0.65, canvas.height * 0.5);
            ctx.scale(1.1, 1);
            ctx.fillText('晗', 0, 0);
            ctx.restore();
            
            // 添加粗体装饰
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.moveTo(canvas.width * 0.2, canvas.height * 0.3);
            ctx.lineTo(canvas.width * 0.8, canvas.height * 0.3);
            ctx.stroke();
            
            ctx.beginPath();
            ctx.moveTo(canvas.width * 0.2, canvas.height * 0.7);
            ctx.lineTo(canvas.width * 0.8, canvas.height * 0.7);
            ctx.stroke();
        }
        
        // 现代风格
        function drawModernStyle() {
            ctx.font = 'bold 70px "Arial", sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            
            // 绘制"王"字
            ctx.save();
            ctx.translate(canvas.width * 0.35, canvas.height * 0.5);
            ctx.rotate(-0.1);
            ctx.fillText('王', 0, 0);
            ctx.restore();
            
            // 绘制"晗"字
            ctx.save();
            ctx.translate(canvas.width * 0.65, canvas.height * 0.5);
            ctx.rotate(0.1);
            ctx.fillText('晗', 0, 0);
            ctx.restore();
            
            // 添加现代感装饰
            ctx.lineWidth = 1;
            for(let i = 0; i < 5; i++) {
                ctx.beginPath();
                ctx.moveTo(canvas.width * 0.2 + i*15, canvas.height * 0.2);
                ctx.lineTo(canvas.width * 0.8 - i*15, canvas.height * 0.8);
                ctx.stroke();
            }
        }
        
        // 古典风格
        function drawClassicStyle() {
            ctx.font = 'bold 85px "隶书", "LiSu", "华文隶书", serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            
            // 绘制"王"字
            ctx.save();
            ctx.translate(canvas.width * 0.35, canvas.height * 0.5);
            ctx.fillText('王', 0, 0);
            ctx.restore();
            
            // 绘制"晗"字
            ctx.save();
            ctx.translate(canvas.width * 0.65, canvas.height * 0.5);
            ctx.fillText('晗', 0, 0);
            ctx.restore();
            
            // 添加古典装饰
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.arc(canvas.width * 0.5, canvas.height * 0.5, 120, 0, Math.PI * 2);
            ctx.stroke();
            
            ctx.beginPath();
            ctx.arc(canvas.width * 0.5, canvas.height * 0.5, 100, 0, Math.PI * 2);
            ctx.stroke();
        }
        
        // 样式按钮事件
        document.querySelectorAll('.style-btn').forEach(button => {
            button.addEventListener('click', function() {
                document.querySelectorAll('.style-btn').forEach(btn => {
                    btn.classList.remove('active');
                });
                this.classList.add('active');
                currentStyle = this.getAttribute('data-style');
                drawSignature();
            });
        });
        
        // 颜色选择事件
        document.querySelectorAll('.color-option').forEach(option => {
            option.addEventListener('click', function() {
                document.querySelectorAll('.color-option').forEach(opt => {
                    opt.classList.remove('active');
                });
                this.classList.add('active');
                currentColor = this.getAttribute('data-color');
                drawSignature();
            });
        });
        
        // 保存签名
        document.getElementById('saveBtn').addEventListener('click', function() {
            const link = document.createElement('a');
            link.download = '王晗艺术签名.png';
            link.href = canvas.toDataURL('image/png');
            link.click();
        });
        
        // 重置签名
        document.getElementById('resetBtn').addEventListener('click', function() {
            currentStyle = 'elegant';
            currentColor = '#000000';
            
            document.querySelectorAll('.style-btn').forEach(btn => {
                btn.classList.remove('active');
                if(btn.getAttribute('data-style') === currentStyle) {
                    btn.classList.add('active');
                }
            });
            
            document.querySelectorAll('.color-option').forEach(opt => {
                opt.classList.remove('active');
                if(opt.getAttribute('data-color') === currentColor) {
                    opt.classList.add('active');
                }
            });
            
            drawSignature();
        });
        
        // 初始化绘制
        drawSignature();
        
        // 响应窗口大小变化
        window.addEventListener('resize', function() {
            drawSignature();
        });
    </script>
</body>
</html>
