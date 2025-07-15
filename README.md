<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario de Tarjeta de Crédito</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }

        .payment-section {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            border-radius: 8px;
            padding: 24px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .payment-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 24px;
        }

        .payment-header .icon {
            width: 20px;
            height: 20px;
            background-color: #28a745;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .payment-header .icon::after {
            content: '✓';
            color: white;
            font-size: 12px;
            font-weight: bold;
        }

        .payment-title {
            font-size: 16px;
            font-weight: 600;
            color: #333;
        }

        .card-icons {
            display: flex;
            gap: 8px;
            margin-bottom: 24px;
        }

        .card-icon {
            width: 40px;
            height: 25px;
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            color: white;
        }

        .visa {
            background: linear-gradient(45deg, #1a1f71, #0066cc);
        }

        .mastercard {
            background: linear-gradient(45deg, #eb001b, #f79e1b);
        }

        .amex {
            background: linear-gradient(45deg, #006fcf, #0099cc);
        }

        .mobile-payment-section {
            max-width: 600px;
            margin: 20px auto 0;
            background: white;
            border-radius: 8px;
            padding: 24px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .mobile-payment-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 24px;
        }

        .mobile-payment-header .icon {
            width: 20px;
            height: 20px;
            background-color: #28a745;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .mobile-payment-header .icon::after {
            content: '📱';
            font-size: 12px;
        }

        .mobile-payment-title {
            font-size: 16px;
            font-weight: 600;
            color: #333;
        }

        .mobile-payment-options {
            display: flex;
            gap: 16px;
            margin-bottom: 24px;
        }

        .mobile-payment-option {
            flex: 1;
            border: 2px solid #e9ecef;
            border-radius: 8px;
            padding: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .mobile-payment-option:hover {
            border-color: #007bff;
            background-color: #f8f9ff;
        }

        .mobile-payment-option.active {
            border-color: #007bff;
            background-color: #f0f8ff;
        }

        .mobile-icon {
            width: 60px;
            height: 40px;
            margin: 0 auto 12px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
            color: white;
        }

        .yape {
            background: linear-gradient(45deg, #722F8F, #9B4DCA);
        }

        .plin {
            background: linear-gradient(45deg, #0066CC, #0088FF);
        }

        .mobile-payment-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 4px;
        }

        .mobile-payment-desc {
            font-size: 12px;
            color: #666;
        }

        .qr-section {
            background: #f8f9fa;
            border: 1px solid #e9ecef;
            border-radius: 8px;
            padding: 24px;
            text-align: center;
            display: none;
        }

        .qr-section.active {
            display: block;
        }

        .qr-code {
            width: 200px;
            height: 200px;
            margin: 0 auto 16px;
            background: white;
            border: 1px solid #ddd;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .qr-pattern {
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(45deg, #000 25%, transparent 25%, transparent 75%, #000 75%, #000),
                linear-gradient(45deg, #000 25%, transparent 25%, transparent 75%, #000 75%, #000);
            background-size: 20px 20px;
            background-position: 0 0, 10px 10px;
            opacity: 0.8;
        }

        .qr-logo {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 40px;
            height: 40px;
            background: white;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .qr-instructions {
            color: #666;
            font-size: 14px;
            line-height: 1.5;
        }

        .qr-instructions strong {
            color: #333;
        }

        .security-icon {
            margin-left: auto;
            width: 30px;
            height: 30px;
            background-color: #f8f9fa;
            border: 1px solid #dee2e6;
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .security-icon::after {
            content: '🔒';
            font-size: 14px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
            font-size: 14px;
        }

        .form-input {
            width: 100%;
            padding: 12px 16px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            transition: border-color 0.3s ease;
            background: #f8f9fa;
        }

        .form-input:focus {
            outline: none;
            border-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0,123,255,0.1);
        }

        .card-number-input {
            position: relative;
        }

        .card-number-input .form-input {
            padding-left: 50px;
        }

        .card-icon-input {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            width: 30px;
            height: 20px;
            background: #f0f0f0;
            border-radius: 3px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 8px;
            color: #666;
        }

        .card-icon-input::after {
            content: '💳';
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }

        .security-group {
            position: relative;
        }

        .security-help {
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(-50%);
            width: 20px;
            height: 20px;
            background: #f8f9fa;
            border: 1px solid #ddd;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 12px;
            color: #666;
        }

        .security-help::after {
            content: '?';
        }

        .checkbox-group {
            display: flex;
            align-items: flex-start;
            gap: 12px;
            margin-top: 24px;
        }

        .checkbox {
            width: 18px;
            height: 18px;
            border: 2px solid #ddd;
            border-radius: 3px;
            cursor: pointer;
            flex-shrink: 0;
            margin-top: 2px;
        }

        .checkbox-label {
            font-size: 14px;
            color: #333;
            line-height: 1.5;
            cursor: pointer;
        }

        .checkbox-description {
            font-size: 13px;
            color: #666;
            margin-top: 8px;
            line-height: 1.4;
        }

    .checkbox:checked {
        background-color: #007bff;
        border-color: #007bff;
        position: relative;
    }

    .checkbox:checked::after {
        content: '✓';
        color: white;
        font-size: 12px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    /* Botón de Confirmar Compra animado */
    .confirm-button {
        padding: 15px 30px;
        background: linear-gradient(90deg, #00bfff, #0099cc);
        border: none;
        border-radius: 12px;
        color: white;
        font-size: 18px;
        box-shadow: 0 8px 20px rgba(0, 191, 255, 0.4);
        transition: all 0.3s ease;
        cursor: pointer;
        margin: 20px auto;
        display: block;
    }

    .confirm-button:hover {
        background: linear-gradient(90deg, #0099cc, #007acc);
        transform: scale(1.05) translateY(-3px);
        box-shadow: 0 12px 24px rgba(0, 191, 255, 0.5);
    }

    /* Responsive */
    @media (max-width: 600px) {
        .form-row {
            grid-template-columns: 1fr;
        }

        .card-icons {
            justify-content: center;
        }
    }
