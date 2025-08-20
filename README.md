<!DOCTYPE html>
<html>
<head>
    <title>교차로 교통량 시각화</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- Chart.js 라이브러리 CDN 링크 (그래프 생성을 위해 사용됩니다) -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f4f4f4;
            color: #333;
        }
        .chart-container {
            width: 90%; /* 화면 너비에 맞춰 조정 */
            max-width: 900px; /* 최대 너비 설정 */
            background-color: #fff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        h1 {
            color: #2c3e50;
            margin-bottom: 25px;
            font-size: 1.8em;
        }
        canvas {
            width: 100% !important; /* 부모 컨테이너에 맞춰 너비 조정 */
            height: 450px !important; /* 고정 높이 설정으로 일관된 비율 유지 */
        }
    </style>
</head>
<body>
    <div class="chart-container">
        <h1>교차로별 교통량 현황</h1>
        <canvas id="trafficVolumeChart"></canvas>
    </div>

    <script>
        // 사용자께서 제공해주신 원본 JSON 데이터입니다.
        const jsonData = {"header":{"resultCode":"00","resultMsg":"NORMAL_SERVICE"},"body":{"pageNo":1,"totalCount":6,"numOfRows":100,"items":{"item":[{"volume":"1333","passTime":"0.0","colDate":"2025082019","crossName":"1호회전교차로","speed":"28.6"},{"volume":"1009","passTime":"0.0","colDate":"2025082019","crossName":"올림픽파크회전교차로","speed":"31.0"},{"volume":"977","passTime":"0.0","colDate":"2025082019","crossName":"청량회전교차로","speed":"29.85"},{"volume":"2084","passTime":"0.0","colDate":"2025082019","crossName":"임당사거리","speed":"22.76"},{"volume":"2509","passTime":"0.0","colDate":"2025082019","crossName":"강릉역육거리","speed":"21.76"},{"volume":"2295","passTime":"0.0","colDate":"2025082019","crossName":"옥천오거리","speed":"19.06"}]}}};

        // 데이터에서 교차로 이름(labels)과 교통량(data)을 추출합니다.
        const items = jsonData.body.items.item;
        const crossNames = items.map(item => item.crossName);
        const volumes = items.map(item => parseInt(item.volume, 10)); // 교통량을 정수형으로 변환합니다.

        // 그래프를 그릴 canvas 요소를 가져옵니다.
        const ctx = document.getElementById('trafficVolumeChart').getContext('2d');

        // Chart.js를 사용하여 막대 그래프를 생성합니다.
        new Chart(ctx, {
            type: 'bar', // 막대 그래프 유형
            data: {
                labels: crossNames, // x축 라벨 (교차로 이름)
                datasets: [{
                    label: '교통량 (대)', // 데이터셋 라벨
                    data: volumes, // y축 데이터 (교통량)
                    backgroundColor: [ // 각 막대의 색상
                        'rgba(255, 99, 132, 0.7)',
                        'rgba(54, 162, 235, 0.7)',
                        'rgba(255, 206, 86, 0.7)',
                        'rgba(75, 192, 192, 0.7)',
                        'rgba(153, 102, 255, 0.7)',
                        'rgba(255, 159, 64, 0.7)'
                    ],
                    borderColor: [ // 각 막대의 테두리 색상
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                responsive: true, // 반응형으로 크기 조정
                maintainAspectRatio: false, // canvas의 종횡비 유지를 비활성화하여 높이를 고정할 수 있도록 합니다.
                plugins: {
                    legend: {
                        display: true, // 범례 표시
                        position: 'top', // 범례 위치
                        labels: {
                            font: {
                                size: 14 // 범례 글자 크기
                            }
                        }
                    },
                    tooltip: {
                        callbacks: {
                            label: function(context) {
                                return context.dataset.label + ': ' + context.raw + ' 대'; // 툴팁에 "대" 추가
                            }
                        }
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true, // y축 0부터 시작
                        title: {
                            display: true,
                            text: '교통량 (대)',
                            font: {
                                size: 16
                            }
                        },
                        ticks: {
                            font: {
                                size: 12
                            }
                        }
                    },
                    x: {
                        title: {
                            display: true,
                            text: '교차로 이름',
                            font: {
                                size: 16
                            }
                        },
                        ticks: {
                            font: {
                                size: 12
                            }
                        }
                    }
                }
            }
        });
    </script>
</body>
</html>
