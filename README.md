<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Before & After Image Slider</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
        }

        /* Khung chứa ảnh, bạn có thể chỉnh width và max-width theo ý muốn */
        .comparison-slider {
            position: relative;
            width: 100%;
            max-width: 800px;
            /* Chỉnh tỷ lệ khung hình tùy theo ảnh thật của bạn (ví dụ: 16/9 hoặc 4/3) */
            aspect-ratio: 16 / 9; 
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        /* Định dạng chung cho cả 2 ảnh */
        .comparison-slider img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            pointer-events: none; /* Để không bị lỗi khi click/kéo thanh trượt */
        }

        /* Ảnh Before nằm trên cùng và bị cắt ngang (clip-path) */
        .img-before {
            z-index: 2;
            clip-path: polygon(0 0, 50% 0, 50% 100%, 0 100%);
        }

        /* Ảnh After nằm dưới */
        .img-after {
            z-index: 1;
        }

        /* Thanh trượt vô hình để nhận tương tác vuốt/kéo */
        .slider-input {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            outline: none;
            background: transparent;
            -webkit-appearance: none;
            appearance: none;
            z-index: 10;
            cursor: ew-resize; /* Trỏ chuột hình mũi tên 2 chiều */
        }

        /* Ẩn cục thumb mặc định của thanh trượt */
        .slider-input::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 5px;
            height: 100vh;
            background: transparent;
        }

        /* Đường gạch trắng phân cách 2 ảnh */
        .slider-line {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 50%; /* Khởi tạo ở giữa */
            width: 4px;
            background-color: white;
            z-index: 5;
            transform: translateX(-50%);
            pointer-events: none;
        }

        /* Nút tròn ở giữa vạch chia */
        .slider-button {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 40px;
            height: 40px;
            background-color: white;
            border-radius: 50%;
            z-index: 5;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
            pointer-events: none;
        }

        /* Mũi tên bên trong nút tròn */
        .slider-button::before,
        .slider-button::after {
            content: '';
            width: 0;
            height: 0;
            border-top: 6px solid transparent;
            border-bottom: 6px solid transparent;
        }
        .slider-button::before {
            border-right: 8px solid #333;
            margin-right: 4px;
        }
        .slider-button::after {
            border-left: 8px solid #333;
            margin-left: 4px;
        }
    </style>
</head>
<body>

    <div class="comparison-slider">
        <!-- ẢNH AFTER (SAU): Đặt link ảnh ở dòng dưới -->
        <img src="images (1).jpg" alt="After" class="img-after">
        
        <!-- ẢNH BEFORE (TRƯỚC): Đặt link ảnh ở dòng dưới -->
        <img src="images.jpg" alt="Before" class="img-before" id="img-before">

        <!-- Các thành phần giao diện kéo -->
        <div class="slider-line" id="slider-line"></div>
        <div class="slider-button" id="slider-button"></div>
        
        <!-- Thanh trượt điều khiển (range) -->
        <input type="range" min="0" max="100" value="50" class="slider-input" id="slider-input">
    </div>

    <script>
        // Lấy các element từ HTML
        const sliderInput = document.getElementById('slider-input');
        const imgBefore = document.getElementById('img-before');
        const sliderLine = document.getElementById('slider-line');
        const sliderButton = document.getElementById('slider-button');

        // Lắng nghe sự kiện khi người dùng kéo thanh trượt
        sliderInput.addEventListener('input', (e) => {
            const sliderValue = e.target.value;
            
            // Cập nhật vùng cắt của ảnh Before (thu nhỏ / phóng to theo % thanh trượt)
            imgBefore.style.clipPath = `polygon(0 0, ${sliderValue}% 0, ${sliderValue}% 100%, 0 100%)`;
            
            // Cập nhật vị trí của vạch chia và nút tròn
            sliderLine.style.left = `${sliderValue}%`;
            sliderButton.style.left = `${sliderValue}%`;
        });
    </script>
</body>
</html>
