<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Inteligente de Transporte</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        .header h1 {
            color: #2c3e50;
            font-size: 2.5em;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .header p {
            color: #7f8c8d;
            font-size: 1.1em;
        }

        .dashboard {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        .card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
        }

        .card h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            font-size: 1.5em;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .icon {
            width: 24px;
            height: 24px;
            fill: #667eea;
        }

        .route-item, .station-item {
            background: rgba(103, 126, 234, 0.1);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            border-left: 4px solid #667eea;
            transition: all 0.3s ease;
        }

        .route-item:hover, .station-item:hover {
            background: rgba(103, 126, 234, 0.2);
            transform: translateX(5px);
        }

        .status {
            display: inline-block;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.8em;
            font-weight: bold;
            margin-left: 10px;
        }

        .status.active {
            background: #2ecc71;
            color: white;
        }

        .status.delayed {
            background: #f39c12;
            color: white;
        }

        .status.offline {
            background: #e74c3c;
            color: white;
        }

        .recharge-section {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
        }

        .recharge-form {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        label {
            margin-bottom: 8px;
            font-weight: bold;
            color: #2c3e50;
        }

        input, select {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
        }

        .btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .stat-number {
            font-size: 2.5em;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }

        .stat-label {
            color: #7f8c8d;
            font-size: 0.9em;
        }

        .charging-points {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .charging-point {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }

        .charging-point:hover {
            transform: translateY(-3px);
        }

        .point-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .point-name {
            font-weight: bold;
            color: #2c3e50;
        }

        .point-status {
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.8em;
            font-weight: bold;
        }

        .available {
            background: #d4edda;
            color: #155724;
        }

        .busy {
            background: #fff3cd;
            color: #856404;
        }

        .maintenance {
            background: #f8d7da;
            color: #721c24;
        }

        .real-time-info {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            max-width: 300px;
            z-index: 1000;
        }

        .pulse {
            width: 12px;
            height: 12px;
            background: #2ecc71;
            border-radius: 50%;
            display: inline-block;
            animation: pulse 2s infinite;
            margin-right: 10px;
        }

        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.2); opacity: 0.7; }
            100% { transform: scale(1); opacity: 1; }
        }

        @media (max-width: 768px) {
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .recharge-form {
                grid-template-columns: 1fr;
            }
            
            .real-time-info {
                position: static;
                margin-bottom: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🚌 Control Inteligente de Transporte</h1>
            <p>Seguimiento en tiempo real, rutas optimizadas y puntos de recarga</p>
        </div>

        <div class="real-time-info">
            <h3><span class="pulse"></span>Tiempo Real</h3>
            <p id="currentTime"></p>
            <p><strong>Buses activos:</strong> <span id="activeBuses">24</span></p>
            <p><strong>Puntos operativos:</strong> <span id="activePoints">18/20</span></p>
        </div>

        <div class="stats">
            <div class="stat-card">
                <div class="stat-number">1,247</div>
                <div class="stat-label">Usuarios hoy</div>
            </div>
            <div class="stat-card">
                <div class="stat-number">89.2%</div>
                <div class="stat-label">Puntualidad</div>
            </div>
            <div class="stat-card">
                <div class="stat-number">S/. 15,680</div>
                <div class="stat-label">Recargas del día</div>
            </div>
            <div class="stat-card">
                <div class="stat-number">2.3 min</div>
                <div class="stat-label">Tiempo promedio</div>
            </div>
        </div>

        <div class="dashboard">
            <div class="card">
                <h2>
                    <svg class="icon" viewBox="0 0 24 24">
                        <path d="M12 2L2 7v10c0 5.55 3.84 9.739 9 9.899V19h2v-2.101c5.16-.16 9-4.349 9-9.899V7l-10-5z"/>
                    </svg>
                    Rutas Activas
                </h2>
                <div id="routesList">
                    <div class="route-item">
                        <strong>Ruta 101 - Lima Centro ↔ Miraflores</strong>
                        <span class="status active">Activa</span>
                        <p>Próximo: 3 min | Capacidad: 85%</p>
                    </div>
                    <div class="route-item">
                        <strong>Ruta 205 - Callao ↔ San Isidro</strong>
                        <span class="status delayed">Retraso</span>
                        <p>Próximo: 8 min | Capacidad: 92%</p>
                    </div>
                    <div class="route-item">
                        <strong>Ruta 150 - Surco ↔ La Molina</strong>
                        <span class="status active">Activa</span>
                        <p>Próximo: 5 min | Capacidad: 67%</p>
                    </div>
                </div>
            </div>

            <div class="card">
                <h2>
                    <svg class="icon" viewBox="0 0 24 24">
                        <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z"/>
                    </svg>
                    Estaciones
                </h2>
                <div id="stationsList">
                    <div class="station-item">
                        <strong>Miraflores - Parque Kennedy</strong>
                        <span class="status active">Operativa</span>
                        <p>Usuarios esperando: 15</p>
                    </div>
                    <div class="station-item">
                        <strong>San Isidro - El Olivar</strong>
                        <span class="status active">Operativa</span>
                        <p>Usuarios esperando: 8</p>
                    </div>
                    <div class="station-item">
                        <strong>San Borja - Centro Empresarial</strong>
                        <span class="status offline">Mantenimiento</span>
                        <p>Fuera de servicio</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="recharge-section">
            <h2>
                <svg class="icon" viewBox="0 0 24 24">
                    <path d="M17 2H7C5.9 2 5 2.9 5 4v16c0 1.1.9 2 2 2h10c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 18H7V4h10v16z"/>
                </svg>
                Recarga de Tarjetas
            </h2>
            <div class="recharge-form">
                <div class="form-group">
                    <label for="cardNumber">Recarga</label>
                    <input type="text" id="cardNumber" placeholder="1234 5678 9012 3456">
                </div>

                <div class="form-group">
                    <label for="paymentMethod">Recarga</label>
                    <select id="paymentMethod">
                        <option value="10">10</option>
                        <option value="20">20</option>
                        <option value="30">30</option>
                        <option value="50">50</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="rechargePoint">Punto de Recarga</label>
                    <select id="rechargePoint">
                        <option value="lima">Lima Centro</option>
                        <option value="miraflores">Miraflores</option>
                        <option value="sanisidro">San Isidro</option>
                        <option value="callao">Callao</option>
                        <option value="barranco">Barranco</option>
                        <option value="lamolina">La Molina</option>
                        <option value="surco">Surco</option>
                        <option value="sanborja">San Borja</option>
                    </select>
                </div>
            </div>
            <button class="btn" onclick="processRecharge()">💳 Procesar Recarga</button>
        </div>

        <div class="charging-points">
            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">Lima Centro</div>
                    <div class="point-status available">Disponible</div>
                </div>
                <p>📍 Plaza de Armas - Jr. Unión 123</p>
                <p>⏰ 24 horas</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">Miraflores</div>
                    <div class="point-status busy">En uso</div>
                </div>
                <p>📍 Av. Larco 456 - Parque Kennedy</p>
                <p>⏰ 06:00 - 22:00</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">San Isidro</div>
                    <div class="point-status available">Disponible</div>
                </div>
                <p>📍 Av. Javier Prado 789 - El Olivar</p>
                <p>⏰ 05:00 - 23:00</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">Callao</div>
                    <div class="point-status available">Disponible</div>
                </div>
                <p>📍 Av. Argentina 321 - Plaza Grau</p>
                <p>⏰ 24 horas</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">Barranco</div>
                    <div class="point-status busy">En uso</div>
                </div>
                <p>📍 Av. Grau 654 - Puente de los Suspiros</p>
                <p>⏰ 06:00 - 22:00</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">La Molina</div>
                    <div class="point-status available">Disponible</div>
                </div>
                <p>📍 Av. Javier Prado Este 987 - Camacho</p>
                <p>⏰ 06:00 - 23:00</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">Surco</div>
                    <div class="point-status available">Disponible</div>
                </div>
                <p>📍 Av. Benavides 147 - Óvalo Higuereta</p>
                <p>⏰ 06:00 - 22:00</p>
            </div>

            <div class="charging-point">
                <div class="point-header">
                    <div class="point-name">San Borja</div>
                    <div class="point-status maintenance">Mantenimiento</div>
                </div>
                <p>📍 Av. San Luis 258 - Centro Empresarial</p>
                <p>⏰ Fuera de servicio</p>
            </div>
        </div>
    </div>

    <script>
        // Función para actualizar el tiempo actual
        function updateTime() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('es-PE', { 
                hour: '2-digit', 
                minute: '2-digit', 
                second: '2-digit' 
            });
            document.getElementById('currentTime').textContent = timeString;
        }

        // Función para procesar la recarga
        function processRecharge() {
            const cardNumber = document.getElementById('cardNumber').value;
            const amount = document.getElementById('rechargeAmount').value;
            const paymentMethod = document.getElementById('paymentMethod').value;
            const rechargePoint = document.getElementById('rechargePoint').value;
            
            if (!cardNumber) {
                alert('Por favor, ingresa el número de tarjeta');
                return;
            }
            
            // Simulación de procesamiento
            alert(`Recarga procesada exitosamente!\n\nTarjeta: ${cardNumber}\nMonto: S/. ${amount}.00\nMétodo: ${paymentMethod}\nPunto: ${rechargePoint}`);
            
            // Limpiar el formulario
            document.getElementById('cardNumber').value = '';
        }

        // Inicializar la aplicación
        document.addEventListener('DOMContentLoaded', function() {
            updateTime();
            setInterval(updateTime, 1000);
        });
    </script>
</body>
</html>
