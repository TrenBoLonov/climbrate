<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ClimbRating — рейтинг трасс и райтеров</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            background: #f5f5f5;
            color: #1a1a1a;
            line-height: 1.5;
        }

        .header {
            background: linear-gradient(135deg, #1e3c2c 0%, #2a4a35 100%);
            color: white;
            padding: 2rem 2rem 1.5rem 2rem;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .header h1 {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }

        .header p {
            opacity: 0.9;
            font-size: 1rem;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
        }

        .filters {
            background: white;
            border-radius: 1rem;
            padding: 1rem 1.5rem;
            margin-bottom: 2rem;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            display: flex;
            gap: 1.5rem;
            flex-wrap: wrap;
            align-items: flex-end;
        }

        .filter-group {
            display: flex;
            flex-direction: column;
            gap: 0.4rem;
        }

        .filter-group label {
            font-size: 0.75rem;
            text-transform: uppercase;
            font-weight: 600;
            color: #666;
            letter-spacing: 0.5px;
        }

        select, button {
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            border: 1px solid #ddd;
            background: white;
            font-size: 0.9rem;
            cursor: pointer;
        }

        button {
            background: #2a4a35;
            color: white;
            border: none;
            font-weight: 500;
            transition: background 0.2s;
        }

        button:hover {
            background: #1e3c2c;
        }

        .section {
            margin-bottom: 3rem;
        }

        .section-title {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .section-title span {
            background: #e0e7e0;
            padding: 0.2rem 0.6rem;
            border-radius: 2rem;
            font-size: 0.8rem;
            font-weight: 500;
            color: #2a4a35;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 1.5rem;
        }

        .route-card {
            background: white;
            border-radius: 1rem;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .route-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 8px 24px rgba(0,0,0,0.12);
        }

        .route-photo {
            width: 100%;
            height: 180px;
            background: #e9ecef;
            object-fit: cover;
        }

        .route-info {
            padding: 1rem;
        }

        .route-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 0.5rem;
        }

        .route-name {
            font-weight: 600;
            font-size: 1.1rem;
        }

        .route-grade {
            background: #f0e6d2;
            color: #b85c1a;
            padding: 0.2rem 0.6rem;
            border-radius: 1rem;
            font-size: 0.8rem;
            font-weight: 700;
        }

        .route-meta {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            margin-top: 0.75rem;
            font-size: 0.85rem;
            color: #555;
        }

        .setter-avatar {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            object-fit: cover;
            background: #ccc;
        }

        .sector {
            background: #eef2f5;
            padding: 0.2rem 0.5rem;
            border-radius: 0.5rem;
            font-size: 0.7rem;
            font-weight: 500;
        }

        .setter-card {
            background: white;
            border-radius: 1rem;
            padding: 1rem;
            display: flex;
            align-items: center;
            gap: 1rem;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            transition: transform 0.2s;
        }

        .setter-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(0,0,0,0.1);
        }

        .setter-avatar-large {
            width: 56px;
            height: 56px;
            border-radius: 50%;
            object-fit: cover;
        }

        .setter-info h3 {
            font-size: 1rem;
            font-weight: 600;
        }

        .setter-stats {
            font-size: 0.8rem;
            color: #555;
            margin-top: 0.25rem;
        }

        .footer {
            text-align: center;
            padding: 2rem;
            color: #777;
            font-size: 0.8rem;
            border-top: 1px solid #e0e0e0;
            margin-top: 2rem;
        }

        @media (max-width: 700px) {
            .container {
                padding: 1rem;
            }
            .grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

<div class="header">
    <h1>🧗 ClimbRating</h1>
    <p>Рейтинг трасс и райтеров на основе данных с твоего скалодрома</p>
</div>

<div class="container">
    <div class="filters">
        <div class="filter-group">
            <label>📅 Период</label>
            <select id="periodFilter">
                <option value="week">За эту неделю</option>
                <option value="month" selected>За этот месяц</option>
                <option value="all">За всё время</option>
            </select>
        </div>
        <div class="filter-group">
            <label>🧱 Сектор / рельеф</label>
            <select id="sectorFilter">
                <option value="all">Все сектора</option>
                <option value="slab">Слэб (плита)</option>
                <option value="overhang">Наклон (оверхэнг)</option>
                <option value="cave">Грот / пещера</option>
                <option value="vertical">Вертикаль</option>
            </select>
        </div>
        <button id="applyFilters">Применить фильтр</button>
    </div>

    <div class="section">
        <div class="section-title">
            🔥 Топ трасс по сложности
            <span>Самые хардовые</span>
        </div>
        <div class="grid" id="routesGrid">
            <div style="padding: 2rem; text-align: center;">Загрузка...</div>
        </div>
    </div>

    <div class="section">
        <div class="section-title">
            🏆 Лучшие райтероб
            <span>По количеству трасс</span>
        </div>
        <div class="grid" id="settersGrid">
            <div style="padding: 2rem; text-align: center;">Загрузка...</div>
        </div>
    </div>
</div>

<div class="footer">
    Данные на основе активности скалодрома • Источник: Climbzilla (агрегированные метрики)
</div>

<script>
    const routesData = [
        { id: 1, name: "Змеиный слипер", grade: "7C", sector: "overhang", setterId: 101, photo: "https://images.unsplash.com/photo-1603211854320-6a7e2fea9dd4?w=600&h=400&fit=crop", setterName: "Алексей Г.", setterAvatar: "https://randomuser.me/api/portraits/men/32.jpg", attempts: 23, date: "2025-04-10" },
        { id: 2, name: "Карманный ад", grade: "7B+", sector: "cave", setterId: 102, photo: "https://images.unsplash.com/photo-1522163182402-834f871fd851?w=600&h=400&fit=crop", setterName: "Мария В.", setterAvatar: "https://randomuser.me/api/portraits/women/68.jpg", attempts: 18, date: "2025-04-05" },
        { id: 3, name: "Пальцеломка", grade: "8A", sector: "overhang", setterId: 103, photo: "https://images.unsplash.com/photo-1635322966215-e9544fb0a32a?w=600&h=400&fit=crop", setterName: "Денис К.", setterAvatar: "https://randomuser.me/api/portraits/men/75.jpg", attempts: 42, date: "2025-03-28" },
        { id: 4, name: "Техничный слэб", grade: "6C+", sector: "slab", setterId: 101, photo: "https://images.unsplash.com/photo-1604497436450-6b3e0c1c5a6b?w=600&h=400&fit=crop", setterName: "Алексей Г.", setterAvatar: "https://randomuser.me/api/portraits/men/32.jpg", attempts: 9, date: "2025-04-12" },
        { id: 5, name: "Динамит", grade: "7A", sector: "vertical", setterId: 104, photo: "https://images.unsplash.com/photo-1635322966215-e9544fb0a32a?w=600&h=400&fit=crop", setterName: "Ольга С.", setterAvatar: "https://randomuser.me/api/portraits/women/90.jpg", attempts: 31, date: "2025-04-01" },
        { id: 6, name: "Грот монстр", grade: "7C+", sector: "cave", setterId: 103, photo: "https://images.unsplash.com/photo-1604497436450-6b3e0c1c5a6b?w=600&h=400&fit=crop", setterName: "Денис К.", setterAvatar: "https://randomuser.me/api/portraits/men/75.jpg", attempts: 37, date: "2025-03-20" },
        { id: 7, name: "Рест-машина", grade: "6B", sector: "slab", setterId: 102, photo: "https://images.unsplash.com/photo-1522163182402-834f871fd851?w=600&h=400&fit=crop", setterName: "Мария В.", setterAvatar: "https://randomuser.me/api/portraits/women/68.jpg", attempts: 12, date: "2025-04-08" }
    ];

    const settersStats = [
        { id: 101, name: "Алексей Г.", avatar: "https://randomuser.me/api/portraits/men/32.jpg", routesCount: 12, avgGrade: "6C", topGrade: "7C" },
        { id: 102, name: "Мария В.", avatar: "https://randomuser.me/api/portraits/women/68.jpg", routesCount: 9, avgGrade: "6B+", topGrade: "7B+" },
        { id: 103, name: "Денис К.", avatar: "https://randomuser.me/api/portraits/men/75.jpg", routesCount: 18, avgGrade: "7A", topGrade: "8A" },
        { id: 104, name: "Ольга С.", avatar: "https://randomuser.me/api/portraits/women/90.jpg", routesCount: 6, avgGrade: "6B", topGrade: "7A" }
    ];

    function gradeToNumber(grade) {
        const grades = ["5A","5B","5C","6A","6A+","6B","6B+","6C","6C+","7A","7A+","7B","7B+","7C","7C+","8A","8A+","8B","8B+","8C"];
        let idx = grades.indexOf(grade);
        return idx === -1 ? 0 : idx;
    }

    function getSectorName(sectorId) {
        const map = { slab: "Слэб", overhang: "Наклон", cave: "Грот", vertical: "Вертикаль" };
        return map[sectorId] || sectorId;
    }

    function renderRoutes(period, sector) {
        let filtered = [...routesData];
        
        if (sector !== 'all') {
            filtered = filtered.filter(route => route.sector === sector);
        }
        
        if (period === 'week') {
            const weekAgo = new Date();
            weekAgo.setDate(weekAgo.getDate() - 7);
            filtered = filtered.filter(route => new Date(route.date) >= weekAgo);
        } else if (period === 'month') {
            const monthAgo = new Date();
            monthAgo.setDate(monthAgo.getDate() - 30);
            filtered = filtered.filter(route => new Date(route.date) >= monthAgo);
        }
        
        filtered.sort((a,b) => gradeToNumber(b.grade) - gradeToNumber(a.grade));
        
        const routesGrid = document.getElementById('routesGrid');
        if (filtered.length === 0) {
            routesGrid.innerHTML = '<div style="grid-column:1/-1; text-align:center; padding:2rem;">Нет трасс по выбранным фильтрам</div>';
            return;
        }
        
        routesGrid.innerHTML = filtered.map(route => `
            <div class="route-card">
                <img class="route-photo" src="${route.photo}" alt="${route.name}" loading="lazy" onerror="this.src='https://placehold.co/600x400?text=Фото+трассы'">
                <div class="route-info">
                    <div class="route-header">
                        <div class="route-name">${route.name}</div>
                        <div class="route-grade">${route.grade}</div>
                    </div>
                    <div class="route-meta">
                        <img class="setter-avatar" src="${route.setterAvatar}" onerror="this.src='https://placehold.co/28x28?text=?'">
                        <span>${route.setterName}</span>
                        <span class="sector">${getSectorName(route.sector)}</span>
                    </div>
                </div>
            </div>
        `).join('');
    }
    
    function renderSetters() {
        const sorted = [...settersStats].sort((a,b) => b.routesCount - a.routesCount);
        const settersGrid = document.getElementById('settersGrid');
        settersGrid.innerHTML = sorted.map(setter => `
            <div class="setter-card">
                <img class="setter-avatar-large" src="${setter.avatar}" onerror="this.src='https://placehold.co/56x56?text=?'">
                <div class="setter-info">
                    <h3>${setter.name}</h3>
                    <div class="setter-stats">
                        📍 ${setter.routesCount} трасс • Средняя сложность ${setter.avgGrade} • 🔝 ${setter.topGrade}
                    </div>
                </div>
            </div>
        `).join('');
    }
    
    document.getElementById('applyFilters').addEventListener('click', () => {
        const period = document.getElementById('periodFilter').value;
        const sector = document.getElementById('sectorFilter').value;
        renderRoutes(period, sector);
    });
    
    renderRoutes('month', 'all');
    renderSetters();
</script>
</body>
</html>
