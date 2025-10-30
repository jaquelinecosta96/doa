<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#9b59b6">
    <meta name="description" content="Encontre brechós em Carapicuíba e região">
    <title>🛍️ Moda da Z/O </title>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.css" />
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: #f8f9fa;
        }
        
        .header {
            background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(155, 89, 182, 0.3);
            position: relative;
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .header p {
            font-size: 1.1rem;
            opacity: 0.95;
        }

        .admin-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: white;
            color: #8e44ad;
            padding: 10px 20px;
            border-radius: 25px;
            text-decoration: none;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .admin-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }
        
        .search-container {
            padding: 30px 20px;
            background: white;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .search-box {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
        }
        
        .search-input {
            width: 100%;
            padding: 18px 60px 18px 20px;
            border: 3px solid #9b59b6;
            border-radius: 50px;
            font-size: 1.1rem;
            transition: all 0.3s ease;
        }
        
        .search-input:focus {
            outline: none;
            box-shadow: 0 0 0 4px rgba(155, 89, 182, 0.2);
            transform: translateY(-2px);
        }
        
        .search-btn {
            position: absolute;
            right: 8px;
            top: 50%;
            transform: translateY(-50%);
            background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 50px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .search-btn:hover {
            transform: translateY(-50%) scale(1.05);
            box-shadow: 0 4px 12px rgba(155, 89, 182, 0.4);
        }
        
        .search-tips {
            text-align: center;
            margin-top: 15px;
            color: #666;
            font-size: 0.9rem;
        }
        
        .map-section {
            height: 300px;
            position: relative;
            display: none;
        }
        
        .map-section.show {
            display: block;
        }
        
        #map {
            height: 100%;
            width: 100%;
        }
        
        .results-container {
            padding: 40px 20px;
            display: block; 
        }
        
        .results-container.show {
            display: block;
        }
        
        .results-header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .results-header h2 {
            color: #8e44ad;
            font-size: 2rem;
            margin-bottom: 10px;
        }
        
        .results-count {
            color: #666;
            font-size: 1.1rem;
        }
        
        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 25px;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .brecho-card {
            background: white;
            border-radius: 15px;
            border-left: 5px solid #9b59b6;
            padding: 25px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            transition: all 0.3s ease;
        }
        
        .brecho-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(155, 89, 182, 0.2);
            border-left-width: 8px;
        }

        .card-gallery {
            margin-bottom: 15px;
            border-radius: 10px;
            overflow: hidden;
            position: relative;
            height: 200px;
            background: #f5f5f5;
        }

        .gallery-main {
            width: 100%;
            height: 100%;
            object-fit: cover;
            cursor: pointer;
        }

        .gallery-nav {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 8px;
        }

        .gallery-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: rgba(255,255,255,0.5);
            cursor: pointer;
            transition: all 0.3s;
        }

        .gallery-dot.active {
            background: white;
            width: 30px;
            border-radius: 5px;
        }

        .no-images {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100%;
            color: #999;
            font-size: 3rem;
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: start;
            margin-bottom: 15px;
        }
        
        .card-title {
            font-size: 1.5rem;
            color: #8e44ad;
            font-weight: 700;
        }
        
        .card-rating {
            display: flex;
            align-items: center;
            gap: 5px;
            background: #fff3cd;
            padding: 5px 12px;
            border-radius: 20px;
        }
        
        .stars {
            color: #ffc107;
            font-size: 1rem;
        }
        
        .rating-text {
            font-size: 0.9rem;
            color: #856404;
            font-weight: 600;
        }
        
        .card-info {
            margin-bottom: 15px;
        }
        
        .info-item {
            display: flex;
            align-items: start;
            gap: 10px;
            margin-bottom: 10px;
            color: #555;
            line-height: 1.5;
        }
        
        .info-icon {
            font-size: 1.2rem;
            min-width: 25px;
        }
        
        .info-text {
            flex: 1;
        }
        
        .info-link {
            color: #9b59b6;
            text-decoration: none;
            font-weight: 600;
            transition: all 0.3s ease;
        }
        
        .info-link:hover {
            color: #8e44ad;
            text-decoration: underline;
        }
        
        .card-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #e9ecef;
        }
        
        .tag {
            background: #f3e5f5;
            color: #8e44ad;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
        }
        
        .schedule {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 10px;
            margin-top: 10px;
            font-size: 0.9rem;
        }
        
        .schedule-item {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            color: #555;
        }
        
        .schedule-day {
            font-weight: 600;
        }
        
        .price-range {
            background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
            color: white;
            padding: 10px 15px;
            border-radius: 10px;
            text-align: center;
            font-weight: 700;
            font-size: 1.1rem;
            margin-top: 10px;
        }

        /* Modal de Login */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
        }

        .modal.show {
            display: flex;
        }

        .modal-content {
            background: white;
            padding: 40px;
            border-radius: 15px;
            max-width: 450px;
            width: 90%;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }

        .modal-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .modal-header h2 {
            color: #8e44ad;
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 500;
        }

        .form-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1rem;
        }

        .form-group input:focus {
            outline: none;
            border-color: #9b59b6;
        }

        .btn-primary {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(155, 89, 182, 0.4);
        }

        .close-modal {
            float: right;
            font-size: 2rem;
            font-weight: 700;
            color: #999;
            cursor: pointer;
            line-height: 1;
        }

        .close-modal:hover {
            color: #333;
        }

        /* Dashboard do Brechó */
        .dashboard {
            display: none;
            padding: 40px 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .dashboard.show {
            display: block;
        }

        .dashboard-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .dashboard-header h2 {
            color: #8e44ad;
            font-size: 2rem;
        }

        .btn-logout {
            background: #e74c3c;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
        }

        .upload-section {
            background: white;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.08);
        }

        .upload-section h3 {
            color: #8e44ad;
            margin-bottom: 20px;
        }

        .upload-area {
            border: 3px dashed #9b59b6;
            border-radius: 10px;
            padding: 40px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .upload-area:hover {
            background: #f8f9fa;
            border-color: #8e44ad;
        }

        .upload-area.dragover {
            background: #f3e5f5;
            border-color: #8e44ad;
        }

        #fileInput {
            display: none;
        }

        .preview-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .preview-item {
            position: relative;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .preview-item img {
            width: 100%;
            height: 150px;
            object-fit: cover;
        }

        .remove-img {
            position: absolute;
            top: 5px;
            right: 5px;
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.8rem;
            }
            
            .cards-grid {
                grid-template-columns: 1fr;
            }
            
            .search-input {
                padding: 15px 20px;
                font-size: 1rem;
            }
            
            .search-btn {
                position: relative;
                right: auto;
                top: auto;
                transform: none;
                width: 100%;
                margin-top: 15px;
            }

            .admin-btn {
                position: static;
                display: block;
                margin: 10px auto 0;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🛍️ Moda da Z/O </h1>
            <p>Encontre os melhores brechós da sua região</p>
            <a href="#" class="admin-btn" id="loginBtn">👤 Área do Brechó</a>
        </div>
        
        <!-- Seção de Busca -->
        <div class="search-container" id="searchContainer">
            <div class="search-box">
                <input 
                    type="text" 
                    id="searchInput" 
                    class="search-input" 
                    placeholder="🔍 Busque por nome, bairro, cidade ou características"
                />
                <button class="search-btn" id="searchBtn">Buscar</button>
            </div>
            <div class="search-tips">
                💡 Dica: Tente buscar por "Osasco", "Alphaville", "infantil" ou "grifes"
            </div>
        </div>
        
        <div class="map-section" id="mapSection">
            <div id="map"></div>
        </div>
        
        <div class="results-container" id="resultsContainer">
            <div id="resultsContent" style="display: none;">
                <div class="results-header">
                    <h2>📍 Brechós Encontrados</h2>
                    <p class="results-count" id="resultsCount"></p>
                </div>
                <div class="cards-grid" id="cardsGrid"></div>
            </div>
        </div>

        <!-- Dashboard do Brechó -->
        <div class="dashboard" id="dashboard">
            <div class="dashboard-header">
                <h2>Gerenciar Imagens - <span id="brechoName"></span></h2>
                <button class="btn-logout" id="logoutBtn">Sair</button>
            </div>

            <div class="upload-section">
                <h3>📸 Upload de Fotos das Roupas</h3>
                <div class="upload-area" id="uploadArea">
                    <p style="font-size: 3rem; margin-bottom: 10px;">📷</p>
                    <p style="font-size: 1.2rem; color: #8e44ad; font-weight: 600;">
                        Clique aqui ou arraste fotos
                    </p>
                    <p style="color: #666; margin-top: 10px;">
                        Formatos aceitos: JPG, PNG, GIF (máx. 5MB cada)
                    </p>
                    <input type="file" id="fileInput" accept="image/*" multiple>
                </div>

                <div class="preview-grid" id="previewGrid"></div>
            </div>
        </div>
    </div>

    <!-- Modal de Login -->
    <div class="modal" id="loginModal">
        <div class="modal-content">
            <span class="close-modal" id="closeModal">&times;</span>
            <div class="modal-header">
                <h2>🛍️ Login do Brechó</h2>
                <p>Acesse para gerenciar suas fotos</p>
            </div>
            <form id="loginForm">
                <div class="form-group">
                    <label for="loginEmail">Email</label>
                    <input type="email" id="loginEmail" required>
                </div>
                <div class="form-group">
                    <label for="loginPassword">Senha</label>
                    <input type="password" id="loginPassword" required>
                </div>
                <button type="submit" class="btn-primary">Entrar</button>
            </form>
            <p style="text-align: center; margin-top: 20px; color: #666;">
                Use: usuario@brecho.com / senha123
            </p>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
    
    <script>
        // Base de dados dos brechós
        const brechos = [
            {
                id: 1,
                nome: "Brechó Estilo & Consciência",
                endereco: "Rua das Palmeiras, 456 - Centro",
                cidade: "Carapicuíba",
                bairro: "Centro",
                telefone: "(11) 4184-5678",
                whatsapp: "5511941845678",
                site: "https://www.estiloconciencia.com.br",
                lat: -23.5225,
                lng: -46.8356,
                avaliacao: 4.8,
                totalAvaliacoes: 127,
                horarios: { "Seg-Sex": "09:00 - 18:00", "Sábado": "09:00 - 14:00", "Domingo": "Fechado" },
                precoMin: 10, precoMax: 150,
                aceitaDoacao: "Sim - Troca por peças",
                tiposPecas: "Usadas e novas",
                caracteristicas: ["doações", "troca", "roupa feminina", "roupa masculina", "acessórios"],
                descricao: "Brechó com excelente variedade e peças de qualidade.",
                email: "usuario@brecho.com",
                senha: "senha123",
                imagens: []
            },
            {
                id: 2,
                nome: "Bazar da Comunidade",
                endereco: "Av. Inocêncio Seráfico, 1234 - Vila Caldas",
                cidade: "Carapicuíba",
                bairro: "Vila Caldas",
                telefone: "(11) 4186-3456",
                whatsapp: "5511941863456",
                site: null,
                lat: -23.5189,
                lng: -46.8423,
                avaliacao: 4.5,
                totalAvaliacoes: 89,
                horarios: { "Seg-Sex": "08:00 - 17:00", "Sábado": "08:00 - 12:00", "Domingo": "Fechado" },
                precoMin: 5, precoMax: 80,
                aceitaDoacao: "Sim - Apenas recebe doações",
                tiposPecas: "Apenas usadas",
                caracteristicas: ["doações sem troca", "roupa infantil", "brinquedos", "preços baixos"],
                descricao: "Projeto social que recebe doações.",
                imagens: []
            }
        ];

        let currentUser = null;
        let map = null;
        let markers = [];
        let userMarker = null;

        // Inicializar mapa
        function initializeLeafletMap() {
            if (!map) {
                map = L.map('map').setView([-23.535, -46.840], 12);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                    attribution: '© OpenStreetMap contributors',
                    maxZoom: 19
                }).addTo(map);
                findAndSetUserLocation();
            }
        }

        function findAndSetUserLocation() {
            if ("geolocation" in navigator) {
                navigator.geolocation.getCurrentPosition(
                    (position) => {
                        const lat = position.coords.latitude;
                        const lng = position.coords.longitude;
                        const userLatLng = [lat, lng];
                        map.setView(userLatLng, 14);
                        const userIcon = L.divIcon({
                            className: 'user-marker',
                            html: '<div style="background: #3498db; width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 3px 10px rgba(0,0,0,0.3); display: flex; align-items: center; justify-content: center; font-size: 16px;">👤</div>',
                            iconSize: [36, 36],
                            iconAnchor: [18, 18]
                        });
                        if (userMarker) map.removeLayer(userMarker);
                        userMarker = L.marker(userLatLng, { icon: userIcon }).addTo(map)
                            .bindPopup("<b>Sua Localização Atual</b>").openPopup();
                    },
                    (error) => {
                        console.warn(`Erro na geolocalização: ${error.message}`);
                    }
                );
            }
        }

        function clearMarkers() {
            markers.forEach(marker => map.removeLayer(marker));
            markers = [];
        }

        function createCustomIcon() {
            return L.divIcon({
                className: 'custom-marker',
                html: '<div style="background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%); width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 3px 10px rgba(0,0,0,0.3); display: flex; align-items: center; justify-content: center; font-size: 16px;">🛍️</div>',
                iconSize: [36, 36],
                iconAnchor: [18, 18]
            });
        }

        function addMarker(brecho) {
            const marker = L.marker([brecho.lat, brecho.lng], {
                icon: createCustomIcon()
            }).addTo(map);
            const stars = '⭐'.repeat(Math.floor(brecho.avaliacao));
            const popupContent = `
                <div style="padding: 10px;">
                    <div style="font-size: 1.3rem; color: #8e44ad; font-weight: 700;">${brecho.nome}</div>
                    <div style="margin: 10px 0;">
                        <span style="color: #ffc107;">${stars}</span>
                        <span style="color: #666; margin-left: 5px;">${brecho.avaliacao}</span>
                    </div>
                    <div><strong>📍</strong> ${brecho.endereco}</div>
                    <div><strong>💰</strong> R$${brecho.precoMin} - R$${brecho.precoMax}</div>
                </div>
            `;
            marker.bindPopup(popupContent, {
                className: 'custom-popup',
                maxWidth: 350
            });
            markers.push(marker);
        }

        function createGallery(imagens) {
            if (!imagens || imagens.length === 0) {
                return '<div class="card-gallery"><div class="no-images">📷</div></div>';
            }

            let currentIndex = 0;
            const galleryId = 'gallery-' + Math.random().toString(36).substr(2, 9);

            const html = `
                <div class="card-gallery" id="${galleryId}">
                    <img src="${imagens[0]}" class="gallery-main" data-gallery="${galleryId}">
                    ${imagens.length > 1 ? `
                        <div class="gallery-nav">
                            ${imagens.map((_, i) => `
                                <div class="gallery-dot ${i === 0 ? 'active' : ''}" data-index="${i}" data-gallery="${galleryId}"></div>
                            `).join('')}
                        </div>
                    ` : ''}
                </div>
            `;

            setTimeout(() => {
                const gallery = document.getElementById(galleryId);
                if (gallery && imagens.length > 1) {
                    const img = gallery.querySelector('.gallery-main');
                    const dots = gallery.querySelectorAll('.gallery-dot');

                    img.addEventListener('click', () => {
                        currentIndex = (currentIndex + 1) % imagens.length;
                        img.src = imagens[currentIndex];
                        dots.forEach((dot, i) => {
                            dot.classList.toggle('active', i === currentIndex);
                        });
                    });

                    dots.forEach((dot, index) => {
                        dot.addEventListener('click', (e) => {
                            e.stopPropagation();
                            currentIndex = index;
                            img.src = imagens[currentIndex];
                            dots.forEach((d, i) => {
                                d.classList.toggle('active', i === currentIndex);
                            });
                        });
                    });
                }
            }, 100);

            return html;
        }

        function createCard(brecho) {
            const stars = '⭐'.repeat(Math.floor(brecho.avaliacao));
            const horariosHtml = Object.entries(brecho.horarios)
                .map(([dia, horario]) => `
                    <div class="schedule-item">
                        <span class="schedule-day">${dia}:</span>
                        <span>${horario}</span>
                    </div>
                `).join('');

            const tagsHtml = brecho.caracteristicas
                .map(tag => `<span class="tag">${tag}</span>`)
                .join('');

            return `
                <div class="brecho-card">
                    ${createGallery(brecho.imagens)}
                    
                    <div class="card-header">
                        <h3 class="card-title">${brecho.nome}</h3>
                        <div class="card-rating">
                            <span class="stars">${stars}</span>
                            <span class="rating-text">${brecho.avaliacao}</span>
                        </div>
                    </div>
                    
                    <div class="card-info">
                        <div class="info-item">
                            <span class="info-icon">📍</span>
                            <span class="info-text">${brecho.endereco}<br><strong>${
