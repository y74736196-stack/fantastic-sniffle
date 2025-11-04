<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>통합과학 2단원: 원자부터 pH까지 시뮬레이션 탐구</title>
    <!-- Tailwind CSS CDN 로드 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap" rel="stylesheet">
    <style>
        /* 기본 스타일 설정: 더 깊은 우주 배경, 폰트 Inter */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #050510; /* 매우 어둡고 깊은 배경 */
            color: #f3f4f6; /* 밝은 텍스트 */
            overflow-x: hidden;
        }

        /* 스크롤 섹션 스타일 (애플 스타일 등장 애니메이션) */
        .story-section {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 8vh 5%;
            transition: opacity 1.8s cubic-bezier(0.25, 0.46, 0.45, 0.94), transform 1.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            opacity: 0.1;
            transform: translateY(80px) scale(0.9); /* 초기 상태: 더 아래, 더 작게 */
        }

        /* Intersection Observer가 감지했을 때의 활성화 상태 */
        .story-section.is-visible {
            opacity: 1;
            transform: translateY(0) scale(1);
        }

        /* 제목 및 강조 스타일 */
        .gradient-text {
            background-image: linear-gradient(to right, #8b5cf6, #ec4899); /* 보라-분홍 그라데이션 */
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-fill-color: transparent;
        }
        
        /* 시뮬레이션 결과 비주얼 요소 (네온 효과를 위한 Shadow 추가) */
        .visual-card {
            background: rgba(17, 24, 39, 0.7); /* 어둡고 투명한 배경 */
            backdrop-filter: blur(15px); /* 블러 강화 */
            border: 1px solid rgba(139, 92, 246, 0.3);
            /* 네온 그림자 효과 */
            box-shadow: 0 0 30px rgba(139, 92, 246, 0.5), 0 0 10px rgba(236, 72, 153, 0.3);
            transform-style: preserve-3d;
            transition: transform 0.6s ease-out, box-shadow 0.6s ease-out;
        }

        /* Chemical Bonding Visual (NaCl) - 초기 3D 배치 */
        #bond-visual {
            transform: rotateX(10deg) rotateY(-10deg); /* 초기 비대칭 3D 뷰 */
        }
        .atom {
            transition: transform 0.5s ease;
        }

        /* Acid/Base Visual (pH Indicator) - 색상 애니메이션 */
        .ph-box {
            transition: background-color 0.8s ease;
        }
        .animate-ph {
            animation: ph-pulse 3s infinite alternate;
        }
        @keyframes ph-pulse {
            0% { transform: scale(1); opacity: 0.8; }
            100% { transform: scale(1.05); opacity: 1; }
        }
    </style>
</head>
<body>

<!-- 헤더 (화면 상단 고정) -->
<header class="fixed top-0 left-0 w-full z-10 p-4 bg-transparent backdrop-filter backdrop-blur-md">
    <nav class="max-w-7xl mx-auto flex justify-between items-center">
        <div class="text-xl font-extrabold gradient-text">Matter Lab V.3.0</div>
        <div class="text-sm font-medium space-x-4 text-gray-300">
            <a href="#section-1" class="hover:text-pink-400 transition">화학 결합</a>
            <a href="#section-2" class="hover:text-pink-400 transition">산염기 반응</a>
            <a href="#section-4" class="hover:text-pink-400 transition">Next Plan</a>
        </div>
    </nav>
</header>

<main>
    <!-- 섹션 0: 히어로 섹션 (시작 화면) -->
    <section id="hero" class="story-section min-h-screen pt-20 flex-col text-center is-visible">
        <h1 class="text-7xl md:text-9xl font-extrabold mb-8 tracking-tight gradient-text leading-tight">
            물질의 <br class="sm:hidden"> 근원을 탐하다
        </h1>
        <p class="text-2xl md:text-3xl text-gray-300 max-w-4xl font-light mt-4">
            통합과학 2단원 시뮬레이션 기반 <br><span class="text-purple-400 font-medium">원자 결합</span>부터 <span class="text-pink-400 font-medium">산도 변화</span>까지
        </p>
        <div class="mt-16 animate-pulse">
            <svg class="w-10 h-10 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M19 13l-7 7-7-7"></path></svg>
        </div>
    </section>

    <!-- 섹션 1: 화학 결합 탐구 (제품 상세 뷰 1 - 원자 세계) -->
    <section id="section-1" class="story-section flex-col md:flex-row max-w-7xl mx-auto">
        
        <!-- 시뮬레이션 결과 비주얼 (Na+Cl- 이온 결합) -->
        <div class="md:w-1/2 h-96 md:h-[80vh] flex items-center justify-center p-4 order-1">
            <div id="bond-visual" class="visual-card w-full h-full rounded-3xl flex items-center justify-center relative transform transition duration-1000">
                <div class="absolute text-7xl font-extrabold text-white opacity-10 -top-8 -left-8 select-none">BONDING</div>
                <div class="flex items-center space-x-16">
                    <!-- Na+ 이온 -->
                    <div class="atom p-6 rounded-full bg-blue-600 hover:bg-blue-500 shadow-2xl shadow-blue-500/50" style="transform: translateZ(80px);">
                        <span class="text-5xl font-bold">Na</span><sub class="text-2xl">+</sub>
                    </div>
                    <!-- 결합 에너지 표시 (더 강조된 아이콘) -->
                    <div class="text-7xl text-pink-400 animate-ping duration-1000">
                        <svg class="w-16 h-16" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                    </div>
                    <!-- Cl- 이온 -->
                    <div class="atom p-6 rounded-full bg-green-600 hover:bg-green-500 shadow-2xl shadow-green-500/50" style="transform: translateZ(80px);">
                        <span class="text-5xl font-bold">Cl</span><sub class="text-2xl">-</sub>
                    </div>
                </div>
                <p class="absolute bottom-6 text-gray-400 text-sm font-medium">Na+ + Cl- → NaCl 격자 형성</p>
            </div>
        </div>
        
        <div class="md:w-1/2 p-8 md:p-12 order-2 md:order-2">
            <p class="text-purple-400 text-lg font-semibold mb-3">PART 1. ⚛️ 원자들의 움직임</p>
            <h2 class="text-6xl font-bold mb-8 leading-tight">화학 결합: <br> 물질을 창조하는 원리</h2>
            <div class="space-y-6 text-gray-400 text-lg">
                <p>
                    **[쉽게 이해하기]**
                    시뮬레이션에서 본 이온 결합은 마치 강력한 **'전자 줄다리기'** 같았습니다. 나트륨(Na)이 전자를 잃고 양이온이 되고, 염소(Cl)가 전자를 받아 음이온이 되면서 둘이 **자석처럼 끌어당기는 모습**은 이론보다 훨씬 직관적이었습니다.
                </p>
                <p>
                    **[탐구 성찰]**
                    딱딱한 공식이 아니라, 원자들이 가장 안정적인 상태를 찾기 위해 에너지를 방출하며 결합하는 **'살아있는 과정'**임을 깨달았습니다. 덕분에 소금(NaCl)처럼 우리 주변의 물질들이 왜 특정한 구조를 갖는지 그 근본적인 이유를 알게 되었습니다.
                </p>
            </div>
        </div>
    </section>

    <!-- 브릿지 섹션: 원자에서 매크로로의 전환 -->
    <section class="story-section min-h-[30vh] pt-20 flex-col max-w-5xl mx-auto text-center">
        <div class="p-10 rounded-2xl bg-white/5 backdrop-blur-sm border-2 border-pink-500/30">
            <h3 class="text-3xl md:text-4xl font-light italic leading-relaxed text-gray-200">
                "결합이 만들어낸 분자들이 <br> 이제 <span class="gradient-text font-bold">산성</span>과 <span class="gradient-text font-bold">염기성</span>이라는 <br> 새로운 성질을 드러낸다."
            </h3>
        </div>
    </section>

    <!-- 섹션 2: 산염기 탐구 (제품 상세 뷰 2 - 거시 세계) -->
    <section id="section-2" class="story-section flex-col md:flex-row max-w-7xl mx-auto">
        <div class="md:w-1/2 p-8 md:p-12 order-2 md:order-1">
            <p class="text-pink-400 text-lg font-semibold mb-3">PART 2. 🧪 이온의 농도 차이</p>
            <h2 class="text-6xl font-bold mb-8 leading-tight">산과 염기: <br> 색으로 표현되는 pH의 비밀</h2>
            <div class="space-y-6 text-gray-400 text-lg">
                <p>
                    **[쉽게 이해하기]**
                    산염기 시뮬레이션은 <span class="font-bold text-white">용액 속의 H+와 OH- 이온 농도 차이</span>가 pH 수치로 변환되어 지시약의 색으로 나타나는 과정을 보여주었습니다. 눈으로 pH 1(빨강)과 pH 13(파랑)의 색깔 변화를 직접 보니, 산염기가 단지 맛의 문제가 아님을 명확히 알 수 있었습니다.
                </p>
                <p>
                    **[탐구 성찰]**
                    수소 이온(H+)의 농도가 10배씩 변할 때마다 pH가 1씩 바뀐다는 **로그 스케일 개념**이 색의 미묘한 변화로 쉽게 느껴졌습니다. 이온의 '수'가 곧 물질의 성질을 결정하는 것을 시각적으로 경험했습니다.
                </p>
            </div>
        </div>

        <!-- 시뮬레이션 결과 비주얼 (pH 지시약 변화) -->
        <div class="md:w-1/2 h-96 md:h-[80vh] flex items-center justify-center p-4 order-1 md:order-2">
            <div class="visual-card w-full h-full rounded-3xl p-10 flex flex-col justify-around transform transition duration-1000 hover:rotate-z-3 hover:scale-[1.05]">
                <div class="text-7xl font-extrabold text-white opacity-10 absolute bottom-4 left-4 select-none">ACID/BASE</div>

                <div class="flex flex-col space-y-6 animate-ph">
                    <!-- pH 1 (Acid) -->
                    <div class="flex items-center space-x-6 bg-red-800/20 p-3 rounded-xl border border-red-500/50">
                        <div class="ph-box w-20 h-20 rounded-xl bg-red-600 shadow-xl flex items-center justify-center font-extrabold text-white text-2xl">pH 1</div>
                        <span class="text-red-300 text-xl font-medium">HCl: H+ 이온 초과</span>
                    </div>
                    <!-- pH 7 (Neutral) -->
                    <div class="flex items-center space-x-6 bg-gray-600/20 p-3 rounded-xl border border-yellow-400/50">
                        <div class="ph-box w-20 h-20 rounded-xl bg-yellow-400 shadow-xl flex items-center justify-center font-extrabold text-gray-800 text-2xl">pH 7</div>
                        <span class="text-yellow-300 text-xl font-medium">H₂O: H+ = OH- 균형</span>
                    </div>
                    <!-- pH 13 (Base) -->
                    <div class="flex items-center space-x-6 bg-blue-800/20 p-3 rounded-xl border border-blue-500/50">
                        <div class="ph-box w-20 h-20 rounded-xl bg-blue-600 shadow-xl flex items-center justify-center font-extrabold text-white text-2xl">pH 13</div>
                        <span class="text-blue-300 text-xl font-medium">NaOH: OH- 이온 초과</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 섹션 3: 통합 성찰 및 결론 -->
    <section id="section-3" class="story-section min-h-[60vh] pt-20 flex-col max-w-5xl mx-auto text-center">
        <h2 class="text-6xl font-bold mb-10 gradient-text">통합 분석 및 성찰</h2>
        <div class="max-w-4xl space-y-8 text-gray-300">
            <p class="text-2xl font-light leading-relaxed">
                두 탐구는 **'물질의 안정성'**이라는 하나의 주제로 연결됩니다. 원자는 결합을 통해 안정화되고, 용액은 H+와 OH-의 균형을 통해 중성을 찾아 안정화됩니다. 시뮬레이션은 이 모든 **화학적 움직임**을 쉽게 이해하게 해준 강력한 도구였습니다.
            </p>
            <p class="text-3xl font-medium text-white p-6 rounded-xl bg-purple-900/60 shadow-2xl shadow-purple-500/30">
                화학은 공식이 아니라, 우리 주변의 모든 현상을 설명하는 **가장 기본적인 시스템 언어**였습니다.
            </p>
        </div>
    </section>

    <!-- 섹션 4: 후속 탐구 의지 (콜투액션 느낌) -->
    <section id="section-4" class="story-section min-h-[50vh] pt-20 flex-col max-w-5xl mx-auto text-center bg-gray-900/40 rounded-[3rem] mb-20 shadow-inner shadow-gray-700/50">
        <p class="text-pink-400 text-lg font-semibold mb-4">NEXT STEPS 🚀</p>
        <h2 class="text-5xl font-bold mb-10 text-white">탐구의 끝은 새로운 시작</h2>
        <div class="space-y-6 text-gray-300 max-w-3xl mx-auto">
            <p class="text-xl font-light leading-relaxed">
                이번 시뮬레이션 경험을 바탕으로, 이론을 현실에 적용하는 **'천연 지시약 프로젝트'**를 통해 지식을 더욱 확장하고 싶습니다.
            </p>
            <div class="p-6 rounded-2xl bg-purple-900/80 border border-purple-500/50 shadow-lg">
                <h3 class="text-2xl font-bold text-white mb-2">후속 탐구 의지: 천연 지시약 프로젝트</h3>
                <p class="text-lg">
                    **목표:** 붉은 양배추, 포도 껍질 등의 <span class="text-yellow-300 font-medium">안토시아닌 색소</span>가 pH에 따라 색깔이 변하는 원리를 탐구하고, 이를 이용해 **친환경 pH 측정 도구**를 직접 제작하는 실질적인 화학 실험을 설계하고 실행할 계획입니다.
                </p>
            </div>
        </div>
    </section>
</main>

<footer class="text-center py-8 text-gray-500 text-sm border-t border-gray-700/50">
    제작자: 통합과학 탐구자 | 리포트 버전 V3.0
</footer>

<script>
    document.addEventListener('DOMContentLoaded', () => {
        // Intersection Observer를 사용하여 스크롤 시 섹션 애니메이션 활성화
        const sections = document.querySelectorAll('.story-section');

        const observerOptions = {
            root: null, // 뷰포트를 기준으로 관찰
            rootMargin: '0px',
            threshold: 0.15 // 섹션의 15%가 보일 때
        };

        const observer = new IntersectionObserver((entries, observer) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    // 뷰포트에 들어오면 'is-visible' 클래스 추가하여 애니메이션 실행
                    entry.target.classList.add('is-visible');
                } else {
                    // 뷰포트에서 나가면 다시 숨기기 (반복 애니메이션 효과)
                    entry.target.classList.remove('is-visible');
                }
            });
        }, observerOptions);

        sections.forEach(section => {
            observer.observe(section);
        });

        // 강화된 3D 회전 효과 (Chemical Bonding Visual)
        const bondVisual = document.getElementById('bond-visual');
        if (bondVisual) {
            bondVisual.addEventListener('mousemove', (e) => {
                const rect = bondVisual.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                const centerX = rect.width / 2;
                const centerY = rect.height / 2;

                // 마우스 위치에 따라 회전 각도 계산 (더 과감하게 회전)
                const rotateX = ((y - centerY) / centerY) * 15; // -15deg ~ 15deg
                const rotateY = ((x - centerX) / centerX) * -15; // -15deg ~ 15deg

                bondVisual.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale(1.05)`;
                bondVisual.style.boxShadow = `0 0 40px rgba(139, 92, 246, 0.7), 0 0 15px rgba(236, 72, 153, 0.5)`;
            });

            bondVisual.addEventListener('mouseleave', () => {
                // 마우스가 나가면 원래대로 복귀 (초기 3D 뷰 유지)
                bondVisual.style.transform = 'rotateX(10deg) rotateY(-10deg) scale(1)';
                bondVisual.style.boxShadow = `0 0 30px rgba(139, 92, 246, 0.5), 0 0 10px rgba(236, 72, 153, 0.3)`;
            });
        }
    });
</script>
