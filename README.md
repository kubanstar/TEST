<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Поиск товаров</title>
    <script src="https://unpkg.com/html5-qrcode"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        
        .search-container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .combined-search-fields {
            display: none;
            gap: 10px;
            margin: 15px 0;
            flex-wrap: wrap;
        }
        
        .combined-field {
            flex: 1;
            min-width: 200px;
        }
        
        .combined-field label {
            display: block;
            text-align: left;
            margin-bottom: 5px;
            font-size: 14px;
            color: #333;
            font-weight: bold;
        }
        
        .combined-input {
            width: 100%;
            padding: 12px;
            font-size: 14px;
            border: 2px solid #ddd;
            border-radius: 5px;
            box-sizing: border-box;
        }
        
        .combined-input:focus {
            border-color: #4CAF50;
            outline: none;
        }
        
        .search-input-wrapper {
            position: relative;
            width: 100%;
            margin-bottom: 10px;
        }
        
        .search-input {
            width: 100%;
            padding: 15px 40px 15px 15px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 5px;
            box-sizing: border-box;
        }
        
        .search-input:focus {
            border-color: #4CAF50;
            outline: none;
        }
        
        .clear-search-btn {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: #999;
            font-size: 18px;
            cursor: pointer;
            padding: 5px;
            display: none;
            transition: color 0.3s;
            line-height: 1;
        }
        
        .clear-search-btn:hover {
            color: #ff4444;
        }
        
        .buttons-container {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .search-button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            flex-grow: 1;
            max-width: 200px;
        }
        
        .scan-button {
            background-color: #2196F3;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            flex-grow: 1;
            max-width: 200px;
        }
        
        .scan-button-android {
            display: flex;
        }
        
        .scan-button-ios {
            display: none;
        }
        
        .search-button:hover {
            background-color: #45a049;
        }
        
        .scan-button:hover {
            background-color: #0b7dda;
        }
        
        .search-button:active {
            background-color: #3d8b40;
        }
        
        .scan-button:active {
            background-color: #0a6ebd;
        }
        
        .scan-icon {
            font-size: 18px;
        }
        
        .search-mode-selector {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 15px 0;
            flex-wrap: wrap;
        }
        
        .mode-option {
            display: flex;
            align-items: center;
            cursor: pointer;
        }
        
        .mode-radio {
            margin-right: 8px;
            cursor: pointer;
        }
        
        .mode-label {
            font-size: 14px;
            color: #333;
            cursor: pointer;
        }
        
        .results-container {
            margin-top: 20px;
            display: none;
        }
        
        .product-card {
            background: white;
            padding: 20px;
            margin-bottom: 10px;
            border-radius: 5px;
            box-shadow: 0 1px 5px rgba(0,0,0,0.1);
            border-left: 4px solid #4CAF50;
        }
        
        .product-field {
            margin-bottom: 5px;
        }
        
        .barcode {
            color: #666;
            font-size: 14px;
        }
        
        .multiple-barcodes {
            color: #2196F3;
            cursor: pointer;
            text-decoration: underline;
            font-weight: bold;
        }
        
        .barcode-tooltip {
            display: none;
            position: absolute;
            background: white;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            z-index: 100;
            max-width: 300px;
            font-size: 12px;			
        }
        
        .barcode-list {
            max-height: 150px;
            overflow-y: auto;
        }
        
        .barcode-item {
            padding: 2px 0;
            font-family: monospace;
            border-bottom: 1px solid #f0f0f0;
        }
        
        .article {
            font-weight: bold;
            color: #333;
            display: flex;
            align-items: center;
        }
        
        .name {
            font-size: 16px;
            color: #222;
            margin: 10px 0;
        }
        
        .price-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            margin: 15px 0;
        }

        .price-line {
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        .price-label {
            font-weight: bold;
            color: #333;
        }

        .price-value {
            font-weight: bold;
            color: #e74c3c;
        }

        .discount-price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 16px;
        }
        
        .old-price-container {
            display: flex;
            gap: 8px;
            margin-top: 2px;
            margin-bottom: 10px;
            align-items: center;
            justify-content: center;
        }
        
        .original-price {
            color: #888;
            font-size: 14px;
            text-decoration: line-through;
        }
        
        .discount-percent {
            color: #888;
            font-size: 14px;
        }
        
        .stock-info {
            margin-top: 15px;
            padding: 10px;
            background-color: #f8f9fa;
            border-radius: 5px;
            border-left: 4px solid #2196F3;
        }
        
        .stock-title {
            font-weight: bold;
            color: #333;
            margin-bottom: 8px;
            font-size: 14px;
        }
        
        .stock-item {
            display: flex;
            justify-content: space-between;
            padding: 3px 0;
            font-size: 13px;
            border-bottom: 1px solid #eee;
        }
        
        .stock-name {
            color: #555;
        }
        
        .stock-quantity {
            font-weight: bold;
            text-align: right;
        }
        
        .stock-quantity.positive {
            color: #2e7d32;
        }
        
        .stock-quantity.negative {
            color: #f44336;
        }
        
        .box-coefficient {
            color: #666;
            font-size: 11px;
            margin-left: 5px;
        }
        
        .storage-location {
            margin-top: 10px;
            padding: 8px;
            background-color: #fff3cd;
            border-radius: 5px;
            border-left: 4px solid #ffc107;
            font-size: 13px;
        }
        
        .storage-title {
            font-weight: bold;
            color: #856404;
            margin-bottom: 3px;
        }
        
        .storage-value {
            color: #333;
            font-weight: bold;
        }
        
        .no-results {
            text-align: center;
            padding: 20px;
            color: #666;
            font-style: italic;
        }
        
        .results-count {
            color: #666;
            font-size: 14px;
            margin-bottom: 10px;
            text-align: left;
        }
        
        .search-mode {
            color: #4CAF50;
            font-size: 12px;
            margin-top: 5px;
            text-align: left;
        }
        
        .print-button {
            background: none;
            border: none;
            cursor: pointer;
            padding: 5px;
            border-radius: 50%;
            transition: all 0.3s;
            width: 42px;
            height: 42px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin-left: auto;
        }

        .print-button:hover {
            background-color: #f0f0f0;
            transform: scale(1.1);
        }

        .image-button {
            background: none;
            border: none;
            cursor: pointer;
            padding: 5px;
            margin-left: 10px;
            border-radius: 50%;
            transition: all 0.3s;
            width: 42px;
            height: 42px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .image-button:hover {
            background-color: #f0f0f0;
            transform: scale(1.1);
        }

        .no-image-text {
            color: #999;
            font-size: 12px;
            margin-left: 10px;
            font-style: italic;
        }
        
        .scroll-to-top-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            display: none;
            justify-content: center;
            align-items: center;
            font-size: 32px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            transition: all 0.3s;
            z-index: 1000;
        }
        
        .scroll-to-top-btn:hover {
            background-color: #45a049;
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.25);
        }
        
        .scroll-to-top-btn:active {
            transform: translateY(-1px);
        }
        
        .scroll-to-top-btn.show {
            display: flex;
        }
        
        .print-modal-new {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }
        
        .print-modal-content-new {
            background: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            max-width: 450px;
            width: 90%;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .print-status {
            margin-top: 15px;
            padding: 10px;
            border-radius: 5px;
            display: none;
        }
        
        .print-status.success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .print-status.error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .print-status.info {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }
        
        .price-tag-preview {
            margin: 20px 0;
            border: 2px dashed #ddd;
            border-radius: 8px;
            padding: 15px;
            background: white;
            position: relative;
            overflow: hidden;
            min-height: 200px;
        }
        
        .price-tag-canvas {
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            border: 1px solid #eee;
            border-radius: 5px;
            background: white;
        }
        
        .print-action-btn {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 15px;
            width: 100%;
            max-width: 250px;
        }
        
        .print-action-btn:hover {
            background-color: #218838;
            transform: translateY(-2px);
        }
        
        .print-action-btn:disabled {
            background-color: #6c757d;
            cursor: not-allowed;
            transform: none;
        }
        
        .printer-status {
            padding: 8px 12px;
            border-radius: 5px;
            margin-bottom: 15px;
            font-size: 14px;
            font-weight: bold;
        }
        
        .printer-connected {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .printer-disconnected {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .printer-connecting {
            background-color: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }
        
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .modal-frame {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .video-wrapper {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin: 20px auto;
        }
        
        #cameraVideo {
            width: 100%;
            height: auto;
            border-radius: 8px;
        }
        
        .camera-controls {
            margin-top: 10px;
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .camera-btn {
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            background-color: #2196F3;
            color: white;
            flex: 1;
            min-width: 120px;
        }
        
        .close-modal {
            background-color: #f44336;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }
        
        .scan-box {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            height: 150px;
            border: 3px solid #4CAF50;
            border-radius: 10px;
            z-index: 1;
            pointer-events: none;
        }
        
        .scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background-color: #4CAF50;
            animation: scan 2s linear infinite;
        }
        
        @keyframes scan {
            0% { top: 0; }
            50% { top: 100%; }
            100% { top: 0; }
        }

        .barcode-supported {
            margin-top: 10px;
            font-size: 12px;
            color: #666;
        }

        .barcode-format {
            display: inline-block;
            background: #e8f5e9;
            color: #2e7d32;
            padding: 2px 6px;
            border-radius: 3px;
            margin: 0 2px;
            font-size: 11px;
        }

        .scan-result-frame {
            max-width: 550px;
            animation: successSlide 0.5s ease-out;
        }
        
        @keyframes successSlide {
            0% { transform: translateY(-30px); opacity: 0; }
            100% { transform: translateY(0); opacity: 1; }
        }
        
        .scan-result-title {
            font-size: 24px;
            color: #4CAF50;
            margin-bottom: 20px;
            font-weight: bold;
        }
        
        .scan-result-count {
            color: #666;
            font-size: 14px;
            margin-bottom: 20px;
        }
        
        .scan-result-products {
            max-height: 400px;
            overflow-y: auto;
            margin: 15px 0;
            text-align: left;
        }
        
        .scan-result-card {
            background: #f9f9f9;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 8px;
            border-left: 4px solid #2196F3;
        }
        
        .scan-result-stock {
            margin-top: 10px;
            padding: 8px;
            background-color: #f0f8ff;
            border-radius: 5px;
            border-left: 3px solid #4CAF50;
        }
        
        .scan-result-storage {
            margin-top: 8px;
            padding: 6px;
            background-color: #fff3cd;
            border-radius: 5px;
            border-left: 3px solid #ffc107;
        }
        
        .scan-result-barcodes {
            margin-top: 8px;
            padding: 6px;
            background-color: #e8f5e9;
            border-radius: 5px;
            border-left: 3px solid #4CAF50;
            cursor: pointer;
        }
        
        .scan-result-barcodes-title {
            font-weight: bold;
            color: #2e7d32;
            margin-bottom: 3px;
            font-size: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .scan-result-barcodes-list {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }
        
        .scan-result-barcodes-list.expanded {
            max-height: 200px;
            overflow-y: auto;
        }
        
        .scan-result-barcode-item {
            padding: 2px 0;
            font-family: monospace;
            font-size: 11px;
            border-bottom: 1px solid #d0f0c0;
        }
        
        .scan-result-actions {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .action-btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s;
            flex: 1;
            min-width: 140px;
            text-align: center;
        }
        
        .continue-scan-btn {
            background-color: #2196F3;
            color: white;
        }
        
        .continue-scan-btn:hover {
            background-color: #0b7dda;
            transform: translateY(-2px);
        }
        
        .close-result-btn {
            background-color: #f44336;
            color: white;
        }
        
        .close-result-btn:hover {
            background-color: #d32f2f;
            transform: translateY(-2px);
        }
        
        .scan-price-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            margin: 10px 0;
        }
        
        .scan-price-line {
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        
        .scan-discount-price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 16px;
        }
        
        .scan-original-price {
            color: #888;
            font-size: 14px;
            text-decoration: line-through;
        }
        
        .scan-discount-percent {
            color: #888;
            font-size: 14px;
        }
        
        .scan-old-price-container {
            display: flex;
            gap: 8px;
            margin-top: 2px;
            margin-bottom: 10px;
            align-items: center;
            justify-content: center;
        }
        .dates-modal {
            max-width: 500px;
            animation: successSlide 0.5s ease-out;
        }
        
        .dates-content {
            text-align: left;
            margin: 20px 0;
            max-height: 400px;
            overflow-y: auto;
        }
        
        .date-section {
            margin-bottom: 20px;
        }
        
        .date-section-title {
            font-size: 18px;
            font-weight: bold;
            color: #333;
            margin-bottom: 10px;
            padding-bottom: 5px;
            border-bottom: 2px solid #4CAF50;
        }
        
        .date-item {
            margin-bottom: 8px;
            padding: 8px;
            background-color: #f9f9f9;
            border-radius: 5px;
            border-left: 3px solid #2196F3;
        }
        
        .date-item-row {
            display: flex;
            align-items: center;
        }
        
        .date-item-label {
            font-weight: bold;
            color: #555;
            min-width: 100px;
            font-size: 13px;
        }
        
        .date-item-time {
            font-size: 13px;
            color: #333;
            font-weight: bold;
        }
        
        .no-dates-info {
            text-align: center;
            padding: 20px;
            color: #666;
            font-style: italic;
        }
        
        .dates-header {
            text-align: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #4CAF50;
        }
        
        .current-date-display {
            font-size: 16px;
            color: #2e7d32;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .data-update-container {
            font-size: 18px;
            color: #e74c3c;
            font-weight: bold;
            margin-bottom: 5px;
        }

        /* Стили для iOS сканера */
        .ios-scanner-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            z-index: 3000;
        }
        
        .ios-scanner-content {
            width: 100%;
            height: 100%;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        
        .ios-scanner-container {
            flex: 1;
            position: relative;
            overflow: hidden;
        }
        
        #ios-qr-reader {
            width: 100%;
            height: 100%;
            position: relative;
        }
        
        /* Кастомные стили для Html5-QRCode */
        #ios-html5-qrcode-anchor-scan-type-change,
        #ios-html5qr-code-full-region__scan_region {
            display: none !important;
        }
        
        #ios-qr-reader__scan_region {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            max-width: 400px;
            height: 200px;
            z-index: 3010;
        }
        
        #ios-qr-reader__scan_region img {
            display: none;
        }
        
        #ios-qr-reader__scan_region hr {
            display: none;
        }
        
        .ios-scan-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 3005;
        }
        
        .ios-scan-frame {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            max-width: 400px;
            height: 200px;
            border: 4px solid rgba(76, 175, 80, 0.8);
            border-radius: 15px;
            box-shadow: 0 0 0 1000px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }
        
        .ios-scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 4px;
            background: linear-gradient(90deg, 
                transparent, 
                #4CAF50, 
                transparent);
            animation: ios-scan 2s ease-in-out infinite;
            box-shadow: 0 0 10px #4CAF50;
        }
        
        @keyframes ios-scan {
            0% { top: 0; opacity: 1; }
            50% { top: 100%; opacity: 1; }
            51% { opacity: 0; }
            100% { top: 0; opacity: 0; }
        }
        
        .ios-scanner-info {
            position: absolute;
            top: calc(50% + 120px);
            left: 0;
            width: 100%;
            text-align: center;
            color: white;
            font-size: 16px;
            padding: 0 20px;
            z-index: 3010;
        }
        
        .ios-modal-controls {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            padding: 20px;
            background: rgba(0, 0, 0, 0.8);
            display: flex;
            gap: 12px;
            z-index: 3100;
        }
        
        .ios-modal-btn {
            flex: 1;
            padding: 16px;
            border: none;
            border-radius: 12px;
            font-size: 17px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .ios-modal-btn-danger {
            background: rgba(255, 59, 48, 0.8);
            color: white;
        }
        
        .ios-modal-btn-primary {
            background: rgba(76, 175, 80, 0.8);
            color: white;
        }
        
        .ios-status-message {
            position: absolute;
            top: 20px;
            left: 0;
            width: 100%;
            text-align: center;
            color: white;
            font-size: 18px;
            font-weight: 600;
            padding: 10px;
            background: rgba(0, 0, 0, 0.5);
            z-index: 3100;
            display: none;
        }
        
        .ios-scanned-badge {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(76, 175, 80, 0.95);
            color: white;
            padding: 20px 40px;
            border-radius: 15px;
            font-size: 24px;
            font-weight: bold;
            display: none;
            z-index: 3100;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            animation: ios-badgeAppear 0.5s ease-out;
        }
        
        @keyframes ios-badgeAppear {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            70% { transform: translate(-50%, -50%) scale(1.1); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }
        
        .ios-loader {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 50px;
            height: 50px;
            border: 5px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: #4CAF50;
            animation: ios-spin 1s linear infinite;
            z-index: 3100;
            display: none;
        }
        
        @keyframes ios-spin {
            100% { transform: translate(-50%, -50%) rotate(360deg); }
        }
        
        .ios-permission-hint {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            max-width: 300px;
            z-index: 3100;
            display: none;
        }
        
        .ios-no-camera {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            max-width: 320px;
            z-index: 3100;
            display: none;
        }
        
        /* Стили для iOS Bluetooth печати */
        .ble-printer-list {
            max-height: 300px;
            overflow-y: auto;
            margin: 15px 0;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        
        .ble-printer-item {
            padding: 12px 15px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        
        .ble-printer-item:hover {
            background-color: #f5f5f5;
        }
        
        .ble-printer-item:last-child {
            border-bottom: none;
        }
        
        .ble-printer-name {
            font-weight: bold;
            color: #333;
        }
        
        .ble-printer-id {
            font-size: 11px;
            color: #999;
            margin-top: 3px;
        }
        
        .ble-connect-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 12px;
            cursor: pointer;
        }
        
        .ble-connect-btn:hover {
            background-color: #45a049;
        }
        
        .ble-connect-btn:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        
        .ble-saved-device {
            background-color: #e8f5e9;
            border-left: 3px solid #4CAF50;
        }
    </style>
</head>
<body>
    <div class="search-container">
        <h2>Поиск товаров</h2>
        
        <div class="search-input-wrapper">
            <input type="text" 
                   class="search-input" 
                   id="searchInput" 
                   placeholder="Введите артикул для поиска..."
                   autocomplete="off">
            <button class="clear-search-btn" id="clearSearchBtn" title="Очистить поле поиска">&#10060;</button>
        </div>
        
        <div class="combined-search-fields" id="combinedSearchFields">
            <div class="combined-field">
                <label for="articleInput">Артикул:</label>
                <input type="text" 
                       class="combined-input" 
                       id="articleInput" 
                       placeholder="Часть артикула..."
                       autocomplete="off">
            </div>
            <div class="combined-field">
                <label for="nameInput">Наименование:</label>
                <input type="text" 
                       class="combined-input" 
                       id="nameInput" 
                       placeholder="Часть наименования..."
                       autocomplete="off">
            </div>
            <div class="combined-field">
                <label for="barcodeInput">Штрихкод:</label>
                <input type="text" 
                       class="combined-input" 
                       id="barcodeInput" 
                       placeholder="Часть штрихкода..."
                       autocomplete="off">
            </div>
        </div>
        
        <div class="search-mode-selector">
            <div class="mode-option">
                <input type="radio" id="modeArticle" name="searchMode" class="mode-radio" value="article" checked>
                <label for="modeArticle" class="mode-label">Артикул</label>
            </div>
            <div class="mode-option">
                <input type="radio" id="modeBarcode" name="searchMode" class="mode-radio" value="barcode">
                <label for="modeBarcode" class="mode-label">Штрихкод</label>
            </div>
            <div class="mode-option">
                <input type="radio" id="modeName" name="searchMode" class="mode-radio" value="name">
                <label for="modeName" class="mode-label">Наименование</label>
            </div>
            <div class="mode-option">
                <input type="radio" id="modeCombined" name="searchMode" class="mode-radio" value="combined">
                <label for="modeCombined" class="mode-label">Комбинированный</label>
            </div>
        </div>
        
        <div class="buttons-container">
            <button class="search-button" id="searchButton">Найти</button>
            <button class="scan-button scan-button-android" id="scanButtonAndroid" style="display: none;">
                <span class="scan-icon">&#128247;</span> Сканировать штрихкод
            </button>
            <button class="scan-button scan-button-ios" id="scanButtonIOS" style="display: none;">
                <span class="scan-icon">&#128247;</span> Сканировать штрихкод
            </button>
        </div>

        <div class="barcode-supported">
            <span class="barcode-format" id="current-date"></span>
        </div>

        <div id="printStatus" class="print-status"></div>
        
        <div class="results-container" id="resultsContainer">
            <!-- Результаты поиска будут здесь -->
        </div>
    </div>

    <!-- Кнопка "Наверх" -->
    <button class="scroll-to-top-btn" id="scrollToTopBtn" title="Наверх">&#9650;</button>

    <!-- Модальное окно камеры для Android -->
    <div class="modal-overlay" id="cameraModal">
        <div class="modal-frame">
            <h3>Сканирование штрихкода</h3>
            <div class="video-wrapper" id="videoContainer">
                <div class="scan-box">
                    <div class="scan-line"></div>
                </div>
                <video id="cameraVideo" playsinline></video>
                <div class="camera-controls">
                    <button class="camera-btn" id="stopCamera">Остановить</button>
                </div>
            </div>
            <button class="close-modal" id="closeCameraModal">Закрыть</button>
        </div>
    </div>

    <!-- Модальное окно сканера для iOS -->
    <div class="ios-scanner-modal" id="iosScannerModal">
        <div class="ios-scanner-content">
            <div class="ios-scanner-container">
                <div id="ios-qr-reader"></div>
                
                <div class="ios-scan-overlay">
                    <div class="ios-scan-frame">
                        <div class="ios-scan-line"></div>
                    </div>
                </div>
                
                <div class="ios-scanner-info">
                    Наведите камеру на штрихкод в рамке
                </div>
                
                <div class="ios-status-message" id="iosScannerStatus"></div>
                <div class="ios-loader" id="iosScannerLoader">Загрузка...</div>
                
                <div class="ios-no-camera" id="iosNoCameraMessage">
                    <h3 style="color: #ff3b30; margin-bottom:15px;">Камера недоступна</h3>
                    <p>Ваш браузер не поддерживает доступ к камере или камера заблокирована.</p>
                    <p style="margin-top:15px; font-size:14px; color:#ccc;">
                        Используйте Safari на iOS
                    </p>
                </div>
            </div>
            
            <div class="ios-modal-controls">
                <button class="ios-modal-btn ios-modal-btn-danger" id="closeIOSScanner">
                    ✕ Закрыть сканер
                </button>
                <button class="ios-modal-btn ios-modal-btn-primary" id="switchIOSCamera" style="display: none;">
                    Переключить камеру
                </button>
            </div>
        </div>
    </div>

    <!-- Модальное окно результатов сканирования -->
    <div class="modal-overlay" id="resultModal">
        <div class="modal-frame scan-result-frame">
            <div class="scan-result-products" id="resultProducts">
                <!-- Список товаров будет здесь -->
            </div>
            <div class="scan-result-count" id="resultCount">
                <!-- Количество найденных товаров -->
            </div>
            <div class="scan-result-actions">
                <button class="action-btn continue-scan-btn" id="continueScanBtn">
                    &#128247; Сканировать еще
                </button>
                <button class="action-btn close-result-btn" id="closeResultBtn">
                    Закрыть
                </button>
            </div>
        </div>
    </div>

    <!-- Модальное окно печати для Android (Web Serial) -->
    <div class="print-modal-new" id="printModal">
        <div class="print-modal-content-new">
            <h3>Печать ценника</h3>
            
            <div id="printerStatus" class="printer-status printer-connecting">
                Подключаюсь к принтеру...
            </div>

            <div class="price-tag-preview">
                <canvas id="priceTagPreviewCanvas" class="price-tag-canvas" width="440" height="284"></canvas>
            </div>
            
            <button class="print-action-btn" id="printActionBtn" disabled>
                Распечатать
            </button>
            
            <button class="close-modal" id="closePrintModal" style="margin-top: 15px;">
                Закрыть
            </button>
        </div>
    </div>

    <!-- Модальное окно печати для iOS (Web Bluetooth) -->
    <div class="print-modal-new" id="printModalIOS">
        <div class="print-modal-content-new">
            <h3>Печать ценника (iOS)</h3>
            
            <div id="printerStatusIOS" class="printer-status printer-disconnected">
                Принтер не подключен
            </div>

            <div id="blePrinterListContainer" style="display: none;">
                <p style="font-size: 14px; color: #666; margin-bottom: 10px;">Доступные принтеры:</p>
                <div id="blePrinterList" class="ble-printer-list">
                    <!-- Список принтеров будет здесь -->
                </div>
            </div>

            <div id="connectedPrinterInfoIOS" style="display: none; margin-bottom: 15px; padding: 10px; background-color: #e8f5e9; border-radius: 8px;">
                <div style="font-weight: bold; color: #2e7d32;">✅ Подключено:</div>
                <div id="connectedPrinterNameIOS" style="font-size: 14px; margin-top: 5px;"></div>
            </div>
            
            <div class="price-tag-preview">
                <canvas id="priceTagPreviewCanvasIOS" class="price-tag-canvas" width="440" height="284"></canvas>
            </div>
            
            <div style="display: flex; gap: 10px; margin-top: 15px; flex-wrap: wrap; justify-content: center;">
                <button class="print-action-btn" id="scanPrintersBtnIOS" style="background-color: #2196F3;">
                    🔍 Найти принтер
                </button>
                <button class="print-action-btn" id="printActionBtnIOS" disabled style="background-color: #28a745;">
                    🖨️ Распечатать
                </button>
            </div>
            
            <button class="close-modal" id="closePrintModalIOS" style="margin-top: 15px;">
                Закрыть
            </button>
        </div>
    </div>

    <!-- Модальное окно с датами изменения файлов -->
    <div class="modal-overlay" id="datesModal">
        <div class="modal-frame dates-modal">
            <div class="dates-header">
                <div class="current-date-display" id="modalCurrentDate">Дата обновления: 04.02.2026</div>
                <div class="data-update-container" id="dataUpdateContainer">Данные на : 14:07</div>
            </div>
            <div id="datesContent" class="dates-content">
                <!-- Содержимое будет сгенерировано JavaScript -->
            </div>
            <button class="close-modal" id="closeDatesModal" style="margin-top: 15px;">
                Закрыть
            </button>
        </div>
    </div>

    <script>
        // ===== КОНФИГУРАЦИЯ =====
        const SHOW_WHOLESALE_PLUS = false; // false - скрыть, true - показать строку Оптовая+
 
		// ===== ДАТА =====
        const DATA_UPDATE_DATE = ""; // Будет заполнена AHK скриптом: "03.02.2025"
 
        // ===== ДАТЫ ИЗМЕНЕНИЯ ФАЙЛОВ =====
        // Эти данные будут заполняться AHK скриптом
        const URAL_OFFICE_DATE = ""; // Будет заполнена AHK скриптом: "03.02.2026 14:32"
        const URAL_DATE = ""; // Будет заполнена AHK скриптом: "04.02.2026 8:19"

        // ===== ГЛОБАЛЬНЫЕ ПЕРЕМЕННЫЕ =====
        let stream = null;
        let barcodeDetector = null;
        let scanInterval = null;
        let lastScannedCode = '';
        
        // Переменные для печати (Android - Web Serial)
        let serialPort = null;
        let serialWriter = null;
        let isPrinterConnected = false;
        
        // Переменные для печати (iOS - Web Bluetooth)
        let bluetoothDevice = null;
        let bluetoothCharacteristic = null;
        let isBLEPrinterConnected = false;
        let savedBluetoothDeviceId = null;
        
        // Текущий товар для печати
        let currentProductForPrint = null;
        let currentProductForPrintIOS = null;
        
        // ===== ПЕРЕМЕННЫЕ ДЛЯ iOS СКАНЕРА =====
        let iosHtml5QrCode = null;
        let iosIsScanning = false;
        let iosLastScannedCode = '';
        let iosCurrentFacingMode = 'environment';
        
        // Пример данных
        const productsData = `2002000149572;620-107K;Портмоне + зажим "SOMUCH" мат, цв: черный;570,00;750,00;587,00;;;;;;;;Sk000009622_1;
2002000149589;411;Сумочка на пояс GOLD CORAL кожа, цв.: черный;800,00;1050,00;824,00;;;;;;;;Sk000009623_1;
2002000149596;BL-S018-1;Портмоне+зажим "HETINO" глянец, тонкое, цв: черный;630,00;850,00;649,00;;;;;;;;Sk000009624_1;
2002000149602;ST6749-3;Сумка мужская POLO А5+, цвет: чёрный;940,00;1250,00;968,00;;;;;;;;Sk000009625_1;
2002000149626;S1394-5;Сумка мужская SOMUCH А4+, вертикальная, цвет: чёрный;1600,00;2400,00;1648,00;6;;11-1/B-5.2;;;;;Sk000009627_1;
`;

        function parseStockValue(value) {
            if (!value) return 0;
            const cleanValue = value.toString().replace(/\s/g, '').replace(/\u00A0/g, '');
            return parseInt(cleanValue) || 0;
        }

        function parseFloatValue(value) {
            if (!value) return 0;
            const cleanValue = value.toString().replace(',', '.').replace(/\s/g, '');
            return parseFloat(cleanValue) || 0;
        }

        function formatNumber(num, isPrice = false) {
            if (num === null || num === undefined) return '0';
            
            let number;
            if (typeof num === 'string') {
                const cleanString = num.replace(',', '.').replace(/\s/g, '');
                number = parseFloat(cleanString);
                if (isNaN(number)) return '0';
            } else {
                number = num;
            }
            
            if (isPrice) {
                const rounded = Math.round(number * 100) / 100;
                const hasKopecks = Math.abs(rounded - Math.round(rounded)) > 0.001;
                
                if (hasKopecks) {
                    return rounded.toFixed(2).replace('.', ',').replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
                } else {
                    return Math.round(rounded).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
                }
            }
            
            if (number < 0) {
                return Math.round(number).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
            }
            return Math.round(number).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
        }

        function formatCoefficient(num) {
            return num.toFixed(3).replace('.', ',');
        }

        function parseProductsData(data) {
            const lines = data.trim().split('\n');
            const products = [];
            
            for (const line of lines) {
                if (!line.trim()) continue;
                
                const parts = line.split(';');
                
                let alternativeImageCode = '';
                if (parts.length >= 14) {
                    alternativeImageCode = parts[13].trim();
                }
                
                products.push({
                    barcode: parts[0] || '',
                    article: parts[1] || '',
                    name: parts[2] || '',
                    wholesalePrice: parts[3] || '',
                    retailPrice: parts[4] || '',
                    wholesalePlusPrice: parts[5] || '',
                    stocks: {
                        warehouse1: parseStockValue(parts[6]),
                        warehouse2: parseStockValue(parts[7]),
                        warehouse3: parseStockValue(parts[8]),
                        warehouse4: 0
                    },
                    coefficients: {
                        warehouse1: 0,
                        warehouse2: 0,
                        warehouse3: 0,
                        warehouse4: 0
                    },
                    storageLocation: parts[8] || '',
                    imageCode: parts[9] || '',
                    alternativeImageCode: alternativeImageCode,
                    discountPercent: parts[10] || '',
                    discountPriceOpt: parts[11] || '',
                    discountPriceRetail: parts[12] || '',
                    boxQuantity: ''
                });
            }
            
            return products;
        }

        function createProductKey(product) {
            return `${product.article}|${product.name}|${product.wholesalePrice}|${product.retailPrice}|${product.wholesalePlusPrice}|${product.stocks.warehouse1}|${product.stocks.warehouse2}|${product.discountPercent}|${product.discountPriceOpt}|${product.discountPriceRetail}|${product.storageLocation}|${product.imageCode}|${product.alternativeImageCode}`;
        }

        function groupProductsByKey(products) {
            const groups = {};
            
            products.forEach(product => {
                const key = createProductKey(product);
                
                if (!groups[key]) {
                    groups[key] = {
                        ...product,
                        barcodes: [product.barcode],
                        count: 1
                    };
                } else {
                    groups[key].barcodes.push(product.barcode);
                    groups[key].count++;
                }
            });
            
            return Object.values(groups);
        }

        function formatPriceWithDiscount(product) {
            const hasDiscount = product.discountPercent && product.discountPercent.trim() !== '';
            
            if (!hasDiscount) {
                let html = `
                    <div class="price-container">
                        <div class="price-line">
                            <span class="price-label">Оптовая:</span>
                            <span class="price-value">${product.wholesalePrice} руб.</span>
                        </div>
                        <div class="price-line">
                            <span class="price-label">Розничная:</span>
                            <span class="price-value">${product.retailPrice} руб.</span>
                        </div>
                `;
                
                if (SHOW_WHOLESALE_PLUS) {
                    html += `
                        <div class="price-line">
                            <span class="price-label">Оптовая+:</span>
                            <span class="price-value">${product.wholesalePlusPrice} руб.</span>
                        </div>
                    `;
                }
                
                html += `</div>`;
                return html;
            }
            
            const discountPercent = product.discountPercent;
            const discountPriceOpt = product.discountPriceOpt || product.wholesalePrice;
            const discountPriceRetail = product.discountPriceRetail || product.retailPrice;
            
            let html = `
                <div class="price-container">
                    <div class="price-line">
                        <span class="price-label">Оптовая:</span>
                        <span class="discount-price">${discountPriceOpt} руб.</span>
                    </div>
                    <div class="old-price-container">
                        <span class="original-price">${product.wholesalePrice} руб.</span>
                        <span class="discount-percent">-${discountPercent}% &#128165;</span>
                    </div>
                    <div class="price-line">
                        <span class="price-label">Розничная:</span>
                        <span class="discount-price">${discountPriceRetail} руб.</span>
                    </div>
                    <div class="old-price-container">
                        <span class="original-price">${product.retailPrice} руб.</span>
                        <span class="discount-percent">-${discountPercent}% &#128165;</span>
                    </div>
            `;
            
            if (SHOW_WHOLESALE_PLUS) {
                html += `
                    <div class="price-line">
                        <span class="price-label">Оптовая+:</span>
                        <span class="price-value">${product.wholesalePlusPrice} руб.</span>
                    </div>
                `;
            }
            
            html += `</div>`;
            return html;
        }

        function formatPriceWithDiscountModal(product) {
            const hasDiscount = product.discountPercent && product.discountPercent.trim() !== '';
            
            if (!hasDiscount) {
                let html = `
                    <div class="scan-price-container">
                        <div class="scan-price-line">
                            <span class="price-label">Оптовая:</span>
                            <span class="price-value">${product.wholesalePrice} руб.</span>
                        </div>
                        <div class="scan-price-line">
                            <span class="price-label">Розничная:</span>
                            <span class="price-value">${product.retailPrice} руб.</span>
                        </div>
                `;
                
                if (SHOW_WHOLESALE_PLUS) {
                    html += `
                        <div class="scan-price-line">
                            <span class="price-label">Оптовая+:</span>
                            <span class="price-value">${product.wholesalePlusPrice} руб.</span>
                        </div>
                    `;
                }
                
                html += `</div>`;
                return html;
            }
            
            const discountPercent = product.discountPercent;
            const discountPriceOpt = product.discountPriceOpt || product.wholesalePrice;
            const discountPriceRetail = product.discountPriceRetail || product.retailPrice;
            
            let html = `
                <div class="scan-price-container">
                    <div class="scan-price-line">
                        <span class="price-label">Оптовая:</span>
                        <span class="scan-discount-price">${discountPriceOpt} руб.</span>
                    </div>
                    <div class="scan-old-price-container">
                        <span class="scan-original-price">${product.wholesalePrice} руб.</span>
                        <span class="scan-discount-percent">-${discountPercent}% &#128165;</span>
                    </div>
                    <div class="scan-price-line">
                        <span class="price-label">Розничная:</span>
                        <span class="scan-discount-price">${discountPriceRetail} руб.</span>
                    </div>
                    <div class="scan-old-price-container">
                        <span class="scan-original-price">${product.retailPrice} руб.</span>
                        <span class="scan-discount-percent">-${discountPercent}% &#128165;</span>
                    </div>
            `;
            
            if (SHOW_WHOLESALE_PLUS) {
                html += `
                    <div class="scan-price-line">
                        <span class="price-label">Оптовая+:</span>
                        <span class="price-value">${product.wholesalePlusPrice} руб.</span>
                    </div>
                `;
            }
            
            html += `</div>`;
            return html;
        }

        // ===== ФУНКЦИИ ДЛЯ РАБОТЫ С СЕРИАЛЬНЫМ ПОРТОМ (Android) =====

        function updatePrinterStatus(message, type = 'connecting') {
            const statusEl = document.getElementById('printerStatus');
            if (!statusEl) return;
            statusEl.textContent = message;
            
            statusEl.classList.remove('printer-connected', 'printer-disconnected', 'printer-connecting');
            
            switch(type) {
                case 'connected':
                    statusEl.classList.add('printer-connected');
                    statusEl.innerHTML = '&#9989; ' + message;
                    break;
                case 'disconnected':
                    statusEl.classList.add('printer-disconnected');
                    statusEl.innerHTML = '&#10060; ' + message;
                    break;
                case 'connecting':
                    statusEl.classList.add('printer-connecting');
                    statusEl.innerHTML = '&#9203; ' + message;
                    break;
            }
        }

        async function connectToPrinter() {
            try {
                updatePrinterStatus('Подключаюсь к принтеру...', 'connecting');
                
                if (!navigator.serial) {
                    throw new Error('Ваш браузер не поддерживает Web Serial. Используйте Chrome/Edge 89+');
                }
                
                const ports = await navigator.serial.getPorts();
                
                if (ports.length > 0) {
                    serialPort = ports[0];
                } else {
                    serialPort = await navigator.serial.requestPort();
                }
                
                await serialPort.open({
                    baudRate: 115200,
                    dataBits: 8,
                    stopBits: 1,
                    parity: 'none'
                });
                
                serialWriter = serialPort.writable.getWriter();
                isPrinterConnected = true;
                
                updatePrinterStatus('Принтер подключен', 'connected');
                return true;
                
            } catch (error) {
                console.error('Ошибка подключения к принтеру:', error);
                updatePrinterStatus(`Ошибка: ${error.message}`, 'disconnected');
                return false;
            }
        }

        async function disconnectFromPrinter() {
            try {
                if (serialWriter) {
                    serialWriter.releaseLock();
                    serialWriter = null;
                }
                
                if (serialPort) {
                    await serialPort.close();
                    serialPort = null;
                }
                
                isPrinterConnected = false;
                updatePrinterStatus('Принтер отключен', 'disconnected');
                return true;
                
            } catch (error) {
                console.error('Ошибка отключения:', error);
                return false;
            }
        }

        async function sendRawData(data) {
            if (!isPrinterConnected || !serialWriter) {
                throw new Error('Сначала подключите принтер');
            }
            
            try {
                await serialWriter.write(data);
                return true;
            } catch (error) {
                console.error('Ошибка отправки данных:', error);
                throw error;
            }
        }

        // ===== НОВЫЕ ФУНКЦИИ ДЛЯ iOS (Web Bluetooth) =====

        function updatePrinterStatusIOS(message, type = 'disconnected') {
            const statusEl = document.getElementById('printerStatusIOS');
            if (!statusEl) return;
            statusEl.textContent = message;
            
            statusEl.classList.remove('printer-connected', 'printer-disconnected', 'printer-connecting');
            
            switch(type) {
                case 'connected':
                    statusEl.classList.add('printer-connected');
                    statusEl.innerHTML = '&#9989; ' + message;
                    break;
                case 'disconnected':
                    statusEl.classList.add('printer-disconnected');
                    statusEl.innerHTML = '&#10060; ' + message;
                    break;
                case 'connecting':
                    statusEl.classList.add('printer-connecting');
                    statusEl.innerHTML = '&#9203; ' + message;
                    break;
            }
        }

        // UUID сервисов и характеристик для принтеров Xprinter (обычно используют стандартный UART)
        const PRINTER_SERVICE_UUID = '0000ff00-0000-1000-8000-00805f9b34fb';
        const PRINTER_WRITE_CHARACTERISTIC_UUID = '0000ff01-0000-1000-8000-00805f9b34fb';
        const PRINTER_NOTIFY_CHARACTERISTIC_UUID = '0000ff02-0000-1000-8000-00805f9b34fb';
        
        // Альтернативные UUID (Nordic UART Service - часто используется в BLE принтерах)
        const NORDIC_UART_SERVICE_UUID = '6e400001-b5a3-f393-e0a9-e50e24dcca9e';
        const NORDIC_UART_WRITE_CHARACTERISTIC_UUID = '6e400002-b5a3-f393-e0a9-e50e24dcca9e';
        const NORDIC_UART_NOTIFY_CHARACTERISTIC_UUID = '6e400003-b5a3-f393-e0a9-e50e24dcca9e';

        async function scanForBLEPrinters() {
            try {
                updatePrinterStatusIOS('Поиск принтеров...', 'connecting');
                
                if (!navigator.bluetooth) {
                    throw new Error('Web Bluetooth не поддерживается. Используйте Safari на iOS.');
                }

                const options = {
                    acceptAllDevices: true,
                    optionalServices: [
                        PRINTER_SERVICE_UUID,
                        NORDIC_UART_SERVICE_UUID,
                        '000018f0-0000-1000-8000-00805f9b34fb', // Стандартный принтер
                        '0000ff00-0000-1000-8000-00805f9b34fb'  // Xprinter
                    ]
                };

                const device = await navigator.bluetooth.requestDevice(options);
                
                if (!device) {
                    throw new Error('Принтер не выбран');
                }

                bluetoothDevice = device;
                
                // Сохраняем ID устройства
                savedBluetoothDeviceId = device.id;
                localStorage.setItem('savedPrinterId', device.id);
                localStorage.setItem('savedPrinterName', device.name || 'Xprinter');

                await connectToBLEPrinter(device);
                
                return true;

            } catch (error) {
                console.error('Ошибка поиска BLE принтера:', error);
                updatePrinterStatusIOS(`Ошибка: ${error.message}`, 'disconnected');
                return false;
            }
        }

        async function connectToBLEPrinter(device) {
            try {
                updatePrinterStatusIOS('Подключение к принтеру...', 'connecting');
                
                const server = await device.gatt.connect();
                
                // Пытаемся найти сервис принтера
                let service = null;
                try {
                    service = await server.getPrimaryService(PRINTER_SERVICE_UUID);
                } catch (e) {
                    try {
                        service = await server.getPrimaryService(NORDIC_UART_SERVICE_UUID);
                    } catch (e2) {
                        throw new Error('Не найден сервис принтера');
                    }
                }

                // Пытаемся найти характеристику для записи
                let characteristic = null;
                try {
                    characteristic = await service.getCharacteristic(PRINTER_WRITE_CHARACTERISTIC_UUID);
                } catch (e) {
                    try {
                        characteristic = await service.getCharacteristic(NORDIC_UART_WRITE_CHARACTERISTIC_UUID);
                    } catch (e2) {
                        throw new Error('Не найдена характеристика для печати');
                    }
                }

                bluetoothCharacteristic = characteristic;
                isBLEPrinterConnected = true;
                
                // Обновляем UI
                updatePrinterStatusIOS('Принтер подключен', 'connected');
                document.getElementById('connectedPrinterInfoIOS').style.display = 'block';
                document.getElementById('connectedPrinterNameIOS').textContent = device.name || 'Xprinter XP-P323B';
                document.getElementById('blePrinterListContainer').style.display = 'none';
                document.getElementById('printActionBtnIOS').disabled = false;
                
                return true;

            } catch (error) {
                console.error('Ошибка подключения к BLE принтеру:', error);
                updatePrinterStatusIOS(`Ошибка: ${error.message}`, 'disconnected');
                return false;
            }
        }

        async function disconnectFromBLEPrinter() {
            try {
                if (bluetoothDevice && bluetoothDevice.gatt.connected) {
                    bluetoothDevice.gatt.disconnect();
                }
                
                bluetoothCharacteristic = null;
                isBLEPrinterConnected = false;
                
                updatePrinterStatusIOS('Принтер отключен', 'disconnected');
                document.getElementById('connectedPrinterInfoIOS').style.display = 'none';
                document.getElementById('printActionBtnIOS').disabled = true;
                
                return true;
                
            } catch (error) {
                console.error('Ошибка отключения:', error);
                return false;
            }
        }

        async function sendBLEData(data) {
            if (!isBLEPrinterConnected || !bluetoothCharacteristic) {
                throw new Error('Сначала подключите принтер');
            }
            
            try {
                await bluetoothCharacteristic.writeValue(data);
                return true;
            } catch (error) {
                console.error('Ошибка отправки данных через BLE:', error);
                throw error;
            }
        }

        async function printPriceTagIOS(product) {
            try {
                if (!isBLEPrinterConnected) {
                    // Пробуем восстановить подключение
                    if (savedBluetoothDeviceId) {
                        try {
                            const devices = await navigator.bluetooth.getDevices();
                            const savedDevice = devices.find(d => d.id === savedBluetoothDeviceId);
                            if (savedDevice) {
                                bluetoothDevice = savedDevice;
                                await connectToBLEPrinter(savedDevice);
                            }
                        } catch (e) {
                            console.warn('Не удалось восстановить подключение:', e);
                        }
                    }
                    
                    if (!isBLEPrinterConnected) {
                        throw new Error('Принтер не подключен');
                    }
                }
                
                const canvas = createPriceTagImage(product);
                const bitmap = canvasToEscPosBitmap(canvas);
                const imageCommand = createEscPosImageCommand(bitmap);
                
                const leftMargin = 0;
                
                const fullCommand = new Uint8Array(imageCommand.length + 10);
                
                fullCommand[0] = 0x1B;
                fullCommand[1] = 0x40;
                
                fullCommand[2] = 0x1B;
                fullCommand[3] = 0x6C;
                fullCommand[4] = leftMargin;
                
                fullCommand.set(imageCommand, 5);
                
                const imageEnd = 5 + imageCommand.length;
                fullCommand[imageEnd] = 0x0A;
                fullCommand[imageEnd + 1] = 0x0A;
                
                await sendBLEData(fullCommand);
                return true;
                
            } catch (error) {
                console.error('Ошибка печати через BLE:', error);
                throw error;
            }
        }

        // ===== ФУНКЦИИ ДЛЯ СОЗДАНИЯ И ПЕЧАТИ ЦЕННИКА =====

        function createPriceTagImage(product) {
        const canvas = document.createElement('canvas');
		canvas.width = 440;
		canvas.height = 240;

		const ctx = canvas.getContext('2d');

		ctx.fillStyle = 'white';
		ctx.fillRect(0, 0, canvas.width, canvas.height);

		ctx.fillStyle = 'black';
		ctx.textAlign = 'center';

		const textScale = 1.5;

		const fonts = {
			article: 17 * textScale,
			product: 19 * textScale,
			price: 22 * textScale,
			price2: 21 * textScale,
			date: 16 * textScale,
			company: 18 * textScale
		};

		let yPos = 25 * textScale;
		const lineHeight = 20 * textScale;
		const thickLineHeight = 3;

		ctx.font = `bold ${fonts.article}px "Arial"`;
		ctx.fillText(product.article, canvas.width / 2, 21);
		yPos += 5 * textScale + 5 * textScale;

		ctx.beginPath();
		ctx.moveTo(-8, 28);
		ctx.lineTo(canvas.width - 0, 28);
		ctx.lineWidth = thickLineHeight;
		ctx.stroke();

		ctx.font = `${fonts.product}px "Arial"`;

		const productName = product.name;
		const words = productName.split(' ');

		const maxCharsPerLine = 35;

		let lines = [];
		let currentLine = '';

		if (productName.length <= maxCharsPerLine) {
			lines.push(productName);
		} else {
			let possibleTwoLines = splitIntoTwoLines(productName, maxCharsPerLine);
    
			if (possibleTwoLines && possibleTwoLines.length === 2) {
				lines = possibleTwoLines;
			} else {
				lines = splitIntoThreeLines(productName, maxCharsPerLine);
			}
		}

		lines.forEach(line => {
			ctx.fillText(line.trim(), canvas.width / 2, yPos);
			yPos += lineHeight;
		});

		ctx.beginPath();
		ctx.moveTo(-8, 95);
		ctx.lineTo(canvas.width - 0, 95);
		ctx.lineWidth = thickLineHeight;
		ctx.stroke();
		yPos += 10 * textScale + 5 * textScale;

		ctx.font = `bold ${fonts.price}px "Arial"`;

		const retailPriceFormatted = formatNumber(product.retailPrice, true);
		ctx.fillText(`РОЗ: ${retailPriceFormatted} Руб.`, canvas.width / 2, 127);

		ctx.beginPath();
		ctx.moveTo(-8, 139);
		ctx.lineTo(canvas.width - 0, 139);
		ctx.lineWidth = thickLineHeight;
		ctx.stroke();
		yPos += 10 * textScale + 5 * textScale;

		ctx.font = `${fonts.price2}px "Arial"`;

		const wholesalePriceNum = parseFloatValue(product.wholesalePrice);
		const wholesalePriceFormatted = Math.round(wholesalePriceNum).toString();
		const wholesaleCode = wholesalePriceFormatted.padStart(6, '0');
		ctx.fillText(`АРТ000${wholesaleCode}`, canvas.width / 2, 167);

		ctx.beginPath();
		ctx.moveTo(-8, 173);
		ctx.lineTo(canvas.width - 0, 173);
		ctx.lineWidth = thickLineHeight;
		ctx.stroke();

		const today = new Date();
		const dateStr = `${today.getDate().toString().padStart(2, '0')}.${(today.getMonth()+1).toString().padStart(2, '0')}.${today.getFullYear()}`;
		ctx.font = `bold ${fonts.date}px "Arial"`;
		ctx.fillText(dateStr, canvas.width / 2, 197);

		ctx.beginPath();
		ctx.moveTo(-8, 205);
		ctx.lineTo(canvas.width - 0, 205);
		ctx.lineWidth = thickLineHeight;
		ctx.stroke();

		ctx.font = `${fonts.company}px "Arial"`;
		ctx.fillText('ИП Мааруф Р.', canvas.width / 2, 230);
            
            return canvas;
        }

        function splitIntoTwoLines(text, maxChars) {
            const words = text.split(' ');
            let lines = [];
            let currentLine = '';
            
            const middle = Math.floor(text.length / 2);
            let bestBreakIndex = -1;
            
            for (let i = 0; i < text.length; i++) {
                if (text[i] === ' ' && Math.abs(i - middle) < Math.abs(bestBreakIndex - middle)) {
                    bestBreakIndex = i;
                }
            }
            
            if (bestBreakIndex !== -1) {
                const line1 = text.substring(0, bestBreakIndex).trim();
                const line2 = text.substring(bestBreakIndex + 1).trim();
                
                if (line1.length <= maxChars && line2.length <= maxChars) {
                    return [line1, line2];
                }
            }
            
            return null;
        }

        function splitIntoThreeLines(text, maxChars) {
            const words = text.split(' ');
            let lines = [];
            let currentLine = '';
            
            for (const word of words) {
                if ((currentLine + ' ' + word).length <= maxChars / 2) {
                    if (currentLine) currentLine += ' ';
                    currentLine += word;
                } else {
                    if (currentLine) {
                        lines.push(currentLine);
                    }
                    currentLine = word;
                    
                    if (lines.length === 2) {
                        currentLine = '';
                        const remainingWords = words.slice(words.indexOf(word));
                        lines.push(remainingWords.join(' '));
                        break;
                    }
                }
            }
            
            if (currentLine && lines.length < 3) {
                lines.push(currentLine);
            }
            
            return lines;
        }

        function canvasToEscPosBitmap(canvas) {
            const ctx = canvas.getContext('2d');
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const data = imageData.data;
            const width = canvas.width;
            const height = canvas.height;
            
            const bytesPerLine = Math.ceil(width / 8);
            const bitmap = new Uint8Array(bytesPerLine * height);
            
            for (let y = 0; y < height; y++) {
                for (let x = 0; x < width; x++) {
                    const pixelIndex = (y * width + x) * 4;
                    const r = data[pixelIndex];
                    const g = data[pixelIndex + 1];
                    const b = data[pixelIndex + 2];
                    
                    const isBlack = (r + g + b) < 384;
                    
                    if (isBlack) {
                        const byteIndex = y * bytesPerLine + Math.floor(x / 8);
                        const bitPosition = 7 - (x % 8);
                        bitmap[byteIndex] |= (1 << bitPosition);
                    }
                }
            }
            
            return {
                data: bitmap,
                width: width,
                height: height,
                bytesPerLine: bytesPerLine
            };
        }

        function createEscPosImageCommand(bitmap) {
            const width = bitmap.width;
            const height = bitmap.height;
            const bytesPerLine = bitmap.bytesPerLine;
            
            const command = new Uint8Array(bitmap.data.length + 8);
            
            command[0] = 0x1D;
            command[1] = 0x76;
            command[2] = 0x30;
            command[3] = 0x00;
            
            const xL = bytesPerLine & 0xFF;
            const xH = (bytesPerLine >> 8) & 0xFF;
            command[4] = xL;
            command[5] = xH;
            
            const yL = height & 0xFF;
            const yH = (height >> 8) & 0xFF;
            command[6] = yL;
            command[7] = yH;
            
            command.set(bitmap.data, 8);
            
            return command;
        }

        async function printPriceTag(product) {
            try {
                if (!isPrinterConnected) {
                    const connected = await connectToPrinter();
                    if (!connected) {
                        throw new Error('Не удалось подключиться к принтеру');
                    }
                }
                
                const canvas = createPriceTagImage(product);
                const bitmap = canvasToEscPosBitmap(canvas);
                const imageCommand = createEscPosImageCommand(bitmap);
                
                const leftMargin = 0;
                
                const fullCommand = new Uint8Array(imageCommand.length + 10);
                
                fullCommand[0] = 0x1B;
                fullCommand[1] = 0x40;
                
                fullCommand[2] = 0x1B;
                fullCommand[3] = 0x6C;
                fullCommand[4] = leftMargin;
                
                fullCommand.set(imageCommand, 5);
                
                const imageEnd = 5 + imageCommand.length;
                fullCommand[imageEnd] = 0x0A;
                fullCommand[imageEnd + 1] = 0x0A;
                
                await sendRawData(fullCommand);
                return true;
                
            } catch (error) {
                console.error('Ошибка печати:', error);
                throw error;
            }
        }

        function updatePriceTagPreview(product) {
            const canvas = document.getElementById('priceTagPreviewCanvas');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');
            
            ctx.fillStyle = 'white';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            canvas.width = 440;
            canvas.height = 284;
            
            const scale = 0.7;
            ctx.save();
            ctx.scale(scale, scale);
            
            const previewCanvas = createPriceTagImage(product);
            ctx.drawImage(previewCanvas, 0, 0);
            
            ctx.restore();
        }

        function updatePriceTagPreviewIOS(product) {
            const canvas = document.getElementById('priceTagPreviewCanvasIOS');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');
            
            ctx.fillStyle = 'white';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            canvas.width = 440;
            canvas.height = 284;
            
            const scale = 0.7;
            ctx.save();
            ctx.scale(scale, scale);
            
            const previewCanvas = createPriceTagImage(product);
            ctx.drawImage(previewCanvas, 0, 0);
            
            ctx.restore();
        }

        // ===== ОБНОВЛЕННЫЕ ФУНКЦИИ ДЛЯ ПЕЧАТИ (Android) =====

        function showPrintStatus(message, type = 'info') {
            const statusEl = document.getElementById('printStatus');
            if (!statusEl) return;
            statusEl.textContent = message;
            statusEl.className = 'print-status ' + type;
            statusEl.style.display = 'block';
            
            setTimeout(() => {
                statusEl.style.display = 'none';
            }, 5000);
        }

        async function openPrintModal(product) {
            currentProductForPrint = product;
            document.getElementById('printModal').style.display = 'flex';
            
            updatePriceTagPreview(product);
            
            const printBtn = document.getElementById('printActionBtn');
            printBtn.disabled = true;
            printBtn.textContent = 'Подключаюсь к принтеру...';
            
            try {
                const connected = await connectToPrinter();
                
                if (connected) {
                    printBtn.disabled = false;
                    printBtn.textContent = 'Распечатать';
                } else {
                    printBtn.disabled = true;
                    printBtn.textContent = 'Не удалось подключиться';
                }
            } catch (error) {
                console.error('Ошибка при подключении:', error);
                printBtn.disabled = true;
                printBtn.textContent = 'Ошибка подключения';
            }
        }

        function closePrintModal() {
            document.getElementById('printModal').style.display = 'none';
            
            setTimeout(() => {
                disconnectFromPrinter();
            }, 1000);
        }

        async function handlePrint() {
            if (!currentProductForPrint) {
                showPrintStatus('Товар не выбран', 'error');
                return;
            }
            
            const printBtn = document.getElementById('printActionBtn');
            printBtn.disabled = true;
            printBtn.textContent = 'Печатаю...';
            
            try {
                await printPriceTag(currentProductForPrint);
                showPrintStatus('Ценник успешно отправлен на печать!', 'success');
                
                setTimeout(() => {
                    closePrintModal();
                }, 1500);
                
            } catch (error) {
                console.error('Ошибка печати:', error);
                showPrintStatus('Ошибка печати: ' + error.message, 'error');
                printBtn.disabled = false;
                printBtn.textContent = 'Распечатать';
            }
        }

        // ===== НОВЫЕ ФУНКЦИИ ДЛЯ ПЕЧАТИ НА iOS =====

        async function openPrintModalIOS(product) {
            currentProductForPrintIOS = product;
            document.getElementById('printModalIOS').style.display = 'flex';
            
            updatePriceTagPreviewIOS(product);
            
            const printBtn = document.getElementById('printActionBtnIOS');
            printBtn.disabled = true;
            
            // Проверяем поддержку Web Bluetooth
            if (!navigator.bluetooth) {
                updatePrinterStatusIOS('Web Bluetooth не поддерживается. Используйте Safari.', 'disconnected');
                document.getElementById('scanPrintersBtnIOS').disabled = true;
                return;
            }
            
            // Пытаемся восстановить сохраненное подключение
            const savedId = localStorage.getItem('savedPrinterId');
            if (savedId && !isBLEPrinterConnected) {
                try {
                    const devices = await navigator.bluetooth.getDevices();
                    const savedDevice = devices.find(d => d.id === savedId);
                    if (savedDevice) {
                        bluetoothDevice = savedDevice;
                        await connectToBLEPrinter(savedDevice);
                    }
                } catch (e) {
                    console.warn('Не удалось восстановить подключение:', e);
                }
            }
            
            if (isBLEPrinterConnected) {
                printBtn.disabled = false;
            } else {
                document.getElementById('blePrinterListContainer').style.display = 'block';
            }
        }

        function closePrintModalIOS() {
            document.getElementById('printModalIOS').style.display = 'none';
            
            setTimeout(() => {
                disconnectFromBLEPrinter();
            }, 1000);
        }

        async function handlePrintIOS() {
            if (!currentProductForPrintIOS) {
                showPrintStatus('Товар не выбран', 'error');
                return;
            }
            
            const printBtn = document.getElementById('printActionBtnIOS');
            printBtn.disabled = true;
            printBtn.textContent = 'Печатаю...';
            
            try {
                await printPriceTagIOS(currentProductForPrintIOS);
                showPrintStatus('Ценник успешно отправлен на печать!', 'success');
                
                setTimeout(() => {
                    closePrintModalIOS();
                }, 1500);
                
            } catch (error) {
                console.error('Ошибка печати:', error);
                showPrintStatus('Ошибка печати: ' + error.message, 'error');
                printBtn.disabled = false;
                printBtn.textContent = 'Распечатать';
            }
        }

        async function scanForPrintersIOS() {
            try {
                await scanForBLEPrinters();
            } catch (error) {
                console.error('Ошибка сканирования:', error);
            }
        }

        // ===== ФУНКЦИИ ДЛЯ ОТОБРАЖЕНИЯ ДАТ ФАЙЛОВ =====

		function getFileDatesData() {
			let displayDate = "Дата не указана";
			if (DATA_UPDATE_DATE) {
				if (DATA_UPDATE_DATE.includes(" ")) {
					displayDate = DATA_UPDATE_DATE.split(" ")[0];
				} else {
					displayDate = DATA_UPDATE_DATE;
				}
			}
    
			return {
				currentDate: displayDate,
				files: [
					{
						location: "Обмен",
						items: [
							{
								label: "СКЛАД",
								lastModified: URAL_OFFICE_DATE || "Дата не указана"
							},
							{
								label: "Торговый зал",
								lastModified: URAL_DATE || "Дата не указана"
							}
					]
					}
				]
			};
		}

        function openDatesModal() {
            const data = getFileDatesData();
            const modal = document.getElementById('datesModal');
            const content = document.getElementById('datesContent');
            const modalCurrentDate = document.getElementById('modalCurrentDate');
            const dataUpdateContainer = document.getElementById('dataUpdateContainer');
            
            modalCurrentDate.textContent = `Дата обновления: ${data.currentDate}`;
            
            let updateTime = "00:00";
            
            if (DATA_UPDATE_DATE && DATA_UPDATE_DATE.includes(" ")) {
                const timeMatch = DATA_UPDATE_DATE.match(/\s(\d{2}:\d{2})$/);
                if (timeMatch && timeMatch[1]) {
                    updateTime = timeMatch[1];
                }
            } else {
                const now = new Date();
                updateTime = now.getHours().toString().padStart(2, '0') + ':' + 
                            now.getMinutes().toString().padStart(2, '0');
            }
            
            dataUpdateContainer.textContent = `Данные на : ${updateTime}`;
            
            let html = '';
            
            if (data.files && data.files.length > 0) {
                data.files.forEach(section => {
                    html += `<div class="date-section">
                        <div class="date-section-title">${section.location}:</div>`;
                    
                    section.items.forEach(item => {
                        html += `<div class="date-item">
                            <div class="date-item-row">
                                <div class="date-item-label">${item.label}:</div>
                                <div class="date-item-time">${item.lastModified}</div>
                            </div>
                        </div>`;
                    });
                    
                    html += `</div>`;
                });
            } else {
                html += `<div class="no-dates-info">Информация о датах изменения файлов отсутствует</div>`;
            }
            
            content.innerHTML = html;
            modal.style.display = 'flex';
        }

        function closeDatesModal() {
            document.getElementById('datesModal').style.display = 'none';
        } 

		// ===== ФУНКЦИИ ДЛЯ СКАНИРОВАНИЯ (Android) =====

        function isIOS() {
            return /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;
        }

        function isAndroid() {
            return /Android/.test(navigator.userAgent);
        }

        function isBarcodeDetectorSupported() {
            return ('BarcodeDetector' in window);
        }

        async function initBarcodeDetector() {
            if (!isBarcodeDetectorSupported()) {
                console.warn('BarcodeDetector API не поддерживается в этом браузере');
                return null;
            }
            
            try {
                const formats = await BarcodeDetector.getSupportedFormats();
                const supportedFormats = formats.filter(format => 
                    ['ean_13', 'ean_8', 'upc_a', 'upc_e', 'code_39', 'code_128', 'codabar'].includes(format)
                );
                
                if (supportedFormats.length === 0) {
                    console.warn('Нет поддержки нужных форматов штрихкодов');
                    return null;
                }
                
                return new BarcodeDetector({ formats: supportedFormats });
            } catch (error) {
                console.error('Ошибка инициализации BarcodeDetector:', error);
                return null;
            }
        }

        function isHTTPS() {
            return window.location.protocol === 'https:';
        }

        function isLocalhost() {
            return window.location.hostname === 'localhost' || 
                   window.location.hostname === '127.0.0.1' ||
                   window.location.hostname === '';
        }

        function canUseCamera() {
            return true;
        }

        function setupPlatformUI() {
            const scanButtonAndroid = document.getElementById('scanButtonAndroid');
            const scanButtonIOS = document.getElementById('scanButtonIOS');
            
            if (isIOS()) {
                scanButtonAndroid.style.display = 'none';
                scanButtonIOS.style.display = 'flex';
                searchButton.style.maxWidth = '300px';
            } else if (isAndroid()) {
                scanButtonAndroid.style.display = 'flex';
                scanButtonIOS.style.display = 'none';
                setTimeout(() => {
                    initBarcodeDetector();
                }, 1000);
            } else {
                scanButtonAndroid.style.display = 'flex';
                scanButtonIOS.style.display = 'none';
                setTimeout(() => {
                    initBarcodeDetector();
                }, 1000);
            }
        }

        async function openCamera() {
            try {
                stopCameraStream();
                
                const constraints = {
                    video: {
                        facingMode: 'environment',
                        width: { ideal: 1280 },
                        height: { ideal: 720 }
                    },
                    audio: false
                };
                
                stream = await navigator.mediaDevices.getUserMedia(constraints);
                
                cameraVideo.srcObject = stream;
                cameraModal.style.display = 'flex';
                
                await cameraVideo.play();
                
                if (!barcodeDetector) {
                    barcodeDetector = await initBarcodeDetector();
                }
                
                if (!barcodeDetector) {
                    alert('Ваш браузер не поддерживает прямое сканирование штрихкодов.');
                    stopCameraStream();
                    return;
                }
                
                startBarcodeDetection(barcodeDetector);
                
            } catch (error) {
                console.error('Ошибка доступа к камере:', error);
                alert('Не удалось получить доступ к камере. Пожалуйста, разрешите доступ к камере в настройках браузера.');
            }
        }

        function startBarcodeDetection(detector) {
            const canvas = document.createElement('canvas');
            const context = canvas.getContext('2d');
            
            scanInterval = setInterval(async () => {
                if (cameraVideo.readyState === cameraVideo.HAVE_ENOUGH_DATA) {
                    canvas.width = cameraVideo.videoWidth;
                    canvas.height = cameraVideo.videoHeight;
                    
                    context.drawImage(cameraVideo, 0, 0, canvas.width, canvas.height);
                    
                    try {
                        const barcodes = await detector.detect(canvas);
                        
                        if (barcodes && barcodes.length > 0) {
                            const barcode = barcodes[0];
                            handleScannedCode(barcode.rawValue);
                            return;
                        }
                        
                    } catch (error) {
                        console.error('Ошибка детектирования штрихкода:', error);
                    }
                }
            }, 300);
        }

        function stopCameraStream() {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                stream = null;
            }
            if (scanInterval) {
                clearInterval(scanInterval);
                scanInterval = null;
            }
            cameraVideo.srcObject = null;
        }

        function handleScannedCode(code) {
            if (!code || code.trim().length === 0) return;
            
            stopCameraStream();
            document.getElementById('modeBarcode').checked = true;
            updateSearchUI();
            
            const cleanCode = code.toString().trim();
            searchInput.value = cleanCode;
            updateClearButton();
            
            const results = performSimpleSearch(cleanCode, 'barcode');
            showScanResults(cleanCode, results);
        }

        // ===== НОВЫЕ ФУНКЦИИ ДЛЯ iOS СКАНЕРА =====

        async function openIOSScanner() {
            console.log('Открытие iOS сканера...');
            
            const iosModal = document.getElementById('iosScannerModal');
            iosModal.style.display = 'block';
            
            document.getElementById('iosScannerLoader').style.display = 'block';
            showIOSScannerStatus('Инициализация камеры...');
            
           
            setTimeout(() => {
                initIOSBarcodeScanner();
            }, 300);
        }

// ===== ИСПРАВЛЕННАЯ ФУНКЦИЯ ДЛЯ iOS (iPhone 11/12/13/14/15) =====
function initIOSBarcodeScanner() {
    try {
        if (iosHtml5QrCode && iosIsScanning) {
            iosHtml5QrCode.stop().then(() => {
                iosHtml5QrCode.clear();
                iosHtml5QrCode = null;
            }).catch(() => {
                iosHtml5QrCode = null;
            });
        }

        // КОНФИГУРАЦИЯ: Убираем facingMode из первого параметра!
        const config = {
            fps: 10,
            qrbox: { width: 250, height: 150 },
            rememberLastUsedCamera: true,
            supportedScanTypes: [Html5QrcodeScanType.SCAN_TYPE_CAMERA],
            // ВАЖНО: Передаем constraints сюда, во второй аргумент!
            videoConstraints: {
                width: { min: 640, ideal: 1280, max: 1920 },
                height: { min: 480, ideal: 720, max: 1080 },
                facingMode: { ideal: "environment" }, // ideal, не exact
                // Подсказываем браузеру, что нам нужна камера, которая умеет фокусироваться
                advanced: [{
                    focusMode: "continuous",
                    // Для iOS 17+ можно добавить zoom, но для совместимости оставим ниже
                }]
            }
        };

        iosHtml5QrCode = new Html5Qrcode("ios-qr-reader");

        // ВАЖНО: Первый аргумент - пустой объект!
        iosHtml5QrCode.start(
            { }, // НЕ передаем facingMode сюда! Оставляем пустым.
            config,
            onIOSScanSuccess,
            onIOSScanError
        ).then(() => {
            console.log('iOS сканирование запущено успешно');
            iosIsScanning = true;

            document.getElementById('iosScannerLoader').style.display = 'none';
            document.getElementById('iosNoCameraMessage').style.display = 'none';
            hideIOSScannerStatus();

            setTimeout(() => {
                if (iosHtml5QrCode && iosIsScanning) {
                    try {
                        // 1. Включаем постоянный автофокус
                        iosHtml5QrCode.applyVideoConstraints({
                            focusMode: "continuous"
                        }).then(() => {
                            console.log('Режим фокусировки установлен: continuous');
                        }).catch(e => console.warn('Не удалось установить focusMode:', e));

                        // 2. Для iPhone 11 и новее: увеличиваем масштаб (зум)
                        //    Это компенсирует большую минимальную дистанцию фокуса!
                        //    Значение 2.0 - 3.0 решает проблему "близко - не видит"
                        const isNewIPhone = /iPhone 1[1-9]|iPhone 2[0-9]|iPhone 1[0-9] Pro/.test(navigator.userAgent);
                        if (isNewIPhone) {
                            setTimeout(() => {
                                iosHtml5QrCode.applyVideoConstraints({
                                    advanced: [{ zoom: 2.2 }] // Экспериментально: 2.2 отлично работает на 12 Pro Max
                                }).then(() => {
                                    console.log('Установлен zoom 2.2 для новой камеры');
                                }).catch(e => console.warn('Зум не поддерживается:', e));
                            }, 500); // Немного с задержкой после установки фокуса
                        }

                    } catch (e) {
                        console.warn('Ошибка при настройке камеры:', e);
                    }
                }
            }, 1500); // Даем камере полностью инициализироваться

        }).catch(err => {
            console.error('Ошибка запуска iOS сканера:', err);

            // --- ЗАПАСНОЙ ПЛАН: Пробуем без videoConstraints если не завелось ---
            if (err.toString().includes('Overconstrained') || err.toString().includes('environment')) {
                console.log('Запасной план: без сложных constraints');
                showIOSScannerStatus('Настройка камеры...');

                iosHtml5QrCode.start(
                    { facingMode: "environment" },
                    {
                        fps: 10,
                        qrbox: { width: 250, height: 150 },
                        rememberLastUsedCamera: true
                    },
                    onIOSScanSuccess,
                    onIOSScanError
                ).then(() => {
                    iosIsScanning = true;
                    document.getElementById('iosScannerLoader').style.display = 'none';
                    hideIOSScannerStatus();
                }).catch(err2 => {
                    console.error('Запасной план тоже не сработал:', err2);
                    showIOSNoCameraMessage();
                });
            } else {
                showIOSNoCameraMessage();
            }
        });

    } catch (error) {
        console.error('Критическая ошибка инициализации iOS сканера:', error);
        showIOSNoCameraMessage();
    }
}

        function onIOSScanSuccess(decodedText, decodedResult) {
            console.log('iOS сканирование успешно:', decodedText);
            
            if (iosLastScannedCode === decodedText) {
                return;
            }
            
            iosLastScannedCode = decodedText;

            
            if (iosHtml5QrCode && iosIsScanning) {
                iosHtml5QrCode.stop().then(() => {
                    iosIsScanning = false;
                }).catch(() => {
                    iosIsScanning = false;
                });
            }
				setTimeout(() => {           
                closeIOSScanner();
                
                document.getElementById('modeBarcode').checked = true;
                updateSearchUI();
                
                const cleanCode = decodedText.toString().trim();
                searchInput.value = cleanCode;
                updateClearButton();
                
                const results = performSimpleSearch(cleanCode, 'barcode');
                showScanResults(cleanCode, results);
                
                setTimeout(() => {
                    iosLastScannedCode = '';
                }, 3000);
		}, 5);				
        }

        function onIOSScanError(error) {
            if (!error.includes('NotFoundException') && !error.includes('No multi format readers configured')) {
                console.warn('Ошибка iOS сканирования:', error);
            }
        }

        function showIOSNoCameraMessage() {
            document.getElementById('iosScannerLoader').style.display = 'none';
            document.getElementById('iosNoCameraMessage').style.display = 'block';
            hideIOSScannerStatus();
        }

        function closeIOSScanner() {
            console.log('Закрытие iOS сканера...');
            
            if (iosHtml5QrCode && iosIsScanning) {
                iosHtml5QrCode.stop().then(() => {
                    console.log('iOS сканирование остановлено');
                    iosHtml5QrCode.clear();
                    iosHtml5QrCode = null;
                    iosIsScanning = false;
                }).catch(err => {
                    console.log('Ошибка остановки iOS сканера:', err);
                    iosHtml5QrCode = null;
                    iosIsScanning = false;
                });
            }
            
            document.getElementById('iosScannerModal').style.display = 'none';
            document.getElementById('iosNoCameraMessage').style.display = 'none';
            hideIOSScannerStatus();
            
            iosCurrentFacingMode = 'environment';
        }

        function showIOSScannerStatus(message) {
            const status = document.getElementById('iosScannerStatus');
            status.textContent = message;
            status.style.display = 'block';
        }

        function hideIOSScannerStatus() {
            document.getElementById('iosScannerStatus').style.display = 'none';
        }

        function switchIOSCamera() {
            if (!iosHtml5QrCode || !iosIsScanning) return;
            
            iosCurrentFacingMode = iosCurrentFacingMode === 'environment' ? 'user' : 'environment';
            
            showIOSScannerStatus('Переключение камеры...');
            
            iosHtml5QrCode.stop().then(() => {
                iosHtml5QrCode.clear();
                
                const config = {
                    fps: 10,
                    qrbox: { width: 250, height: 150 },
                    rememberLastUsedCamera: true,
                    supportedScanTypes: [Html5QrcodeScanType.SCAN_TYPE_CAMERA]
                };
                
                iosHtml5QrCode.start(
                    { facingMode: iosCurrentFacingMode },
                    config,
                    onIOSScanSuccess,
                    onIOSScanError
                ).then(() => {
                    hideIOSScannerStatus();
                }).catch(err => {
                    console.error('Ошибка переключения камеры:', err);
                    showIOSScannerStatus('Ошибка переключения камеры');
                });
            }).catch(() => {});
        }

        // ===== ОБЩИЕ ФУНКЦИИ =====

        function showScanResults(code, results) {
            lastScannedCode = code;
            
            if (results.length === 0) {
                resultCount.textContent = 'Товары не найдены';
                resultCount.style.color = '#f44336';
                resultProducts.innerHTML = '<div class="scan-result-card" style="text-align: center; color: #666; font-style: italic;">По этому штрихкоду товары не найдены в базе данных</div>';
            } else {
                const groupedResults = groupProductsByKey(results);
                resultCount.textContent = `Найдено товаров: ${results.length} (${groupedResults.length} уникальных)`;
                resultCount.style.color = '#4CAF50';
                
                resultProducts.innerHTML = '';
                
                groupedResults.forEach(product => {
                    const productCard = document.createElement('div');
                    productCard.className = 'scan-result-card';
                    
                    const hasImage = product.imageCode && product.imageCode.trim() !== '';
                    
                    // Используем соответствующую функцию для создания кнопки печати в зависимости от платформы
                    let printButtonHTML;
                    if (isIOS()) {
                        printButtonHTML = `<button class="print-button" onclick="openPrintModalIOS(${JSON.stringify(product).replace(/"/g, '&quot;')})" title="Печать ценника (iOS)">&#129534;</button>`;
                    } else {
                        printButtonHTML = `<button class="print-button" onclick="openPrintModal(${JSON.stringify(product).replace(/"/g, '&quot;')})" title="Печать ценника">&#129534;</button>`;
                    }

                    productCard.innerHTML = `
                        <div style="font-size: 12px; color: #666; margin-bottom: 5px;">
                            <strong>Штрихкод:</strong> ${product.count > 1 ? `Несколько (${product.count})` : product.barcode}
                        </div>
                        <div style="font-weight: bold; color: #333; margin-bottom: 5px; display: flex; align-items: center; justify-content: space-between;">
                            <div style="display: flex; align-items: center;" id="articleContainer_${product.article.replace(/[^a-zA-Z0-9]/g, '_')}">
                                <strong>Артикул:</strong> ${product.article}
                                ${hasImage ? '<button class="image-button" style="margin-left: 10px;">&#127750;</button>' : '<span class="no-image-text">(без изображения)</span>'}
                            </div>
                            ${printButtonHTML}
                        </div>
                        <div style="font-size: 16px; color: #222; margin-bottom: 8px;">
                            ${product.name}
                        </div>
                        ${formatPriceWithDiscountModal(product)}
                        ${formatStockInfoModal(product)}
                    `;
                    
                    if (hasImage) {
                        setTimeout(() => {
                            const container = productCard.querySelector(`#articleContainer_${product.article.replace(/[^a-zA-Z0-9]/g, '_')}`);
                            if (container) {
                                const imageButton = container.querySelector('.image-button');
                                if (imageButton) {
                                    imageButton.onclick = function() {
                                        showProductImage(product);
                                    };
                                }
                            }
                        }, 0);
                    }
                    
                    resultProducts.appendChild(productCard);
                });
            }
            
            cameraModal.style.display = 'none';
            document.getElementById('iosScannerModal').style.display = 'none';
            resultModal.style.display = 'flex';
        }

        function formatStockInfoModal(product) {
            const stocks = product.stocks;
            const storageLocation = product.storageLocation;
            const barcodes = product.barcodes;
            const scannedCode = lastScannedCode;
            
            let html = '<div class="scan-result-stock">';
            html += '<div style="font-weight: bold; color: #333; margin-bottom: 5px; font-size: 13px;">Остатки:</div>';
            
            html += `<div style="display: flex; justify-content: space-between; padding: 2px 0; font-size: 12px;">
                <span style="color: #555;">ТОРГОВЫЙ ЗАЛ 10-11:</span>
                <span style="color: ${stocks.warehouse1 < 0 ? '#f44336' : '#2e7d32'}; font-weight: bold;">${formatNumber(stocks.warehouse1)} шт.</span>
            </div>`;
            
            html += `<div style="display: flex; justify-content: space-between; padding: 2px 0; font-size: 12px;">
                <span style="color: #555;">СКЛАД кожгалантереи:</span>
                <span style="color: ${stocks.warehouse2 < 0 ? '#f44336' : '#2e7d32'}; font-weight: bold;">${formatNumber(stocks.warehouse2)} шт.</span>
            </div>`;
            
            html += '</div>';
            
            if (barcodes && barcodes.length > 1) {
                html += createBarcodesListHTML(barcodes, scannedCode);
            }
            
            if (storageLocation && storageLocation.trim() !== '') {
                html += `<div class="scan-result-storage">
                    <div style="font-weight: bold; color: #856404; margin-bottom: 3px; font-size: 12px;">Место хранения:</div>
                    <div style="color: #333; font-weight: bold; font-size: 13px;">${storageLocation}</div>
                </div>`;
            }
            
            return html;
        }

        function createMultipleBarcodesHTML(barcodes, query) {
            const uniqueBarcodes = [...new Set(barcodes)];
            const barcodesCount = uniqueBarcodes.length;
            
            let html = `<span class="multiple-barcodes" onclick="showBarcodeTooltip(event, this)">Несколько (${barcodesCount})</span>`;
            html += `<div class="barcode-tooltip">`;
            html += `<div class="barcode-list">`;
            
            uniqueBarcodes.forEach(barcode => {
                const highlightedBarcode = highlightMatch(barcode, query);
                html += `<div class="barcode-item">${highlightedBarcode}</div>`;
            });
            
            html += `</div>`;
            html += `</div>`;
            
            return html;
        }

        function createBarcodesListHTML(barcodes, scannedCode) {
            const uniqueBarcodes = [...new Set(barcodes)];
            const barcodesCount = uniqueBarcodes.length;
            
            let html = `<div class="scan-result-barcodes" onclick="toggleBarcodesList(this)">`;
            html += `<div class="scan-result-barcodes-title">`;
            html += `<span>Штрихкоды (${barcodesCount}):</span>`;
            html += `<span style="font-size: 10px; color: #666;">нажмите для просмотра</span>`;
            html += `</div>`;
            html += `<div class="scan-result-barcodes-list">`;
            
            uniqueBarcodes.forEach(barcode => {
                const isScanned = barcode === scannedCode;
                const barcodeClass = isScanned ? 'style="color: #e74c3c; font-weight: bold;"' : '';
                html += `<div class="scan-result-barcode-item" ${barcodeClass}>${barcode}${isScanned ? ' ?' : ''}</div>`;
            });
            
            html += `</div>`;
            html += `</div>`;
            
            return html;
        }

        function showBarcodeTooltip(event, element) {
            event.stopPropagation();
            
            document.querySelectorAll('.barcode-tooltip').forEach(tooltip => {
                tooltip.style.display = 'none';
            });
            
            const tooltip = element.nextElementSibling;
            if (tooltip && tooltip.classList.contains('barcode-tooltip')) {
                tooltip.style.display = 'block';
                
                const rect = element.getBoundingClientRect();
                tooltip.style.position = 'fixed';
                tooltip.style.left = Math.min(rect.left, window.innerWidth - 320) + 'px';
                tooltip.style.top = (rect.bottom + 5) + 'px';
                
                const closeTooltip = (e) => {
                    if (!tooltip.contains(e.target) && e.target !== element) {
                        tooltip.style.display = 'none';
                        document.removeEventListener('click', closeTooltip);
                    }
                };
                
                setTimeout(() => {
                    document.addEventListener('click', closeTooltip);
                }, 100);
            }
        }

        function toggleBarcodesList(element) {
            const list = element.querySelector('.scan-result-barcodes-list');
            list.classList.toggle('expanded');
            
            const title = element.querySelector('.scan-result-barcodes-title span:last-child');
            if (list.classList.contains('expanded')) {
                title.textContent = 'нажмите для скрытия';
            } else {
                title.textContent = 'нажмите для просмотра';
            }
        }

        function highlightMatch(text, searchTerm) {
            if (!searchTerm || !text) return text;
            const regex = new RegExp(`(${searchTerm.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')})`, 'gi');
            return text.toString().replace(regex, '<mark>$1</mark>');
        }

        // ===== ОСТАЛЬНЫЕ ФУНКЦИИ =====

        function getCurrentSearchMode() {
            const selectedRadio = document.querySelector('input[name="searchMode"]:checked');
            return selectedRadio ? selectedRadio.value : 'article';
        }

        function getSearchModeDisplayName(mode) {
            switch(mode) {
                case 'article': return 'по артикулу';
                case 'barcode': return 'по штрихкоду';
                case 'name': return 'по наименованию';
                case 'combined': return 'комбинированный';
                default: return 'по артикулу';
            }
        }

        function performCombinedSearch(articlePart, namePart, barcodePart) {
            const products = parseProductsData(productsData);
            return products.filter(product => {
                let matches = 0;
                let totalConditions = 0;
                
                if (articlePart && articlePart.trim() !== '') {
                    totalConditions++;
                    if (product.article.toLowerCase().includes(articlePart.toLowerCase())) {
                        matches++;
                    }
                }
                
                if (namePart && namePart.trim() !== '') {
                    totalConditions++;
                    if (product.name.toLowerCase().includes(namePart.toLowerCase())) {
                        matches++;
                    }
                }
                
                if (barcodePart && barcodePart.trim() !== '') {
                    totalConditions++;
                    if (product.barcode.includes(barcodePart)) {
                        matches++;
                    }
                }
                
                return totalConditions > 0 && matches === totalConditions;
            });
        }

        function performSimpleSearch(searchTerm, mode) {
            const products = parseProductsData(productsData);
            return products.filter(product => {
                switch(mode) {
                    case 'article':
                        return product.article.toLowerCase().includes(searchTerm.toLowerCase());
                    
                    case 'barcode':
                        return product.barcode.includes(searchTerm);
                    
                    case 'name':
                        return product.name.toLowerCase().includes(searchTerm.toLowerCase());
                    
                    default:
                        return product.article.toLowerCase().includes(searchTerm.toLowerCase());
                }
            });
        }

        function createProductCard(product, query, searchMode) {
            const productCard = document.createElement('div');
            productCard.className = 'product-card';
            
            let highlightedName = product.name;
            let highlightedArticle = product.article;
            let highlightedBarcode = '';
            
            if (searchMode === 'комбинированный') {
                if (query.article) {
                    highlightedArticle = highlightMatch(product.article, query.article);
                }
                if (query.name) {
                    highlightedName = highlightMatch(product.name, query.name);
                }
                if (query.barcode) {
                    if (product.count > 1) {
                        highlightedBarcode = createMultipleBarcodesHTML(product.barcodes, query.barcode);
                    } else {
                        highlightedBarcode = highlightMatch(product.barcode, query.barcode);
                    }
                }
            } else {
                if (searchMode === 'по артикулу' || searchMode === 'комбинированный') {
                    highlightedArticle = highlightMatch(product.article, query);
                }
                if (searchMode === 'по наименованию' || searchMode === 'комбинированный') {
                    highlightedName = highlightMatch(product.name, query);
                }
                if (searchMode === 'по штрихкоду') {
                    if (product.count > 1) {
                        highlightedBarcode = createMultipleBarcodesHTML(product.barcodes, query);
                    } else {
                        highlightedBarcode = highlightMatch(product.barcode, query);
                    }
                }
            }
            
            if (!highlightedBarcode) {
                if (product.count > 1) {
                    highlightedBarcode = createMultipleBarcodesHTML(product.barcodes, '');
                } else {
                    highlightedBarcode = product.barcode;
                }
            }
            
            const container = document.createElement('div');
            
            const articleRow = document.createElement('div');
            articleRow.className = 'article';
            articleRow.innerHTML = `Артикул: ${highlightedArticle}`;
            
            const hasImage = product.imageCode && product.imageCode.trim() !== '';
            
            if (hasImage) {
                const imageButton = document.createElement('button');
                imageButton.className = 'image-button';
                imageButton.title = 'Показать изображение товара';
                imageButton.innerHTML = '&#127750;';
                imageButton.onclick = function() {
                    showProductImage(product);
                };
                articleRow.appendChild(imageButton);
            } else {
                const noImageSpan = document.createElement('span');
                noImageSpan.className = 'no-image-text';
                noImageSpan.textContent = '(без изображения)';
                articleRow.appendChild(noImageSpan);
            }
            
            // Используем соответствующую функцию для создания кнопки печати в зависимости от платформы
            let printButton;
            if (isIOS()) {
                printButton = document.createElement('button');
                printButton.className = 'print-button';
                printButton.title = 'Печать ценника (iOS)';
                printButton.innerHTML = '&#129534;';
                printButton.onclick = function() {
                    openPrintModalIOS(product);
                };
            } else {
                printButton = document.createElement('button');
                printButton.className = 'print-button';
                printButton.title = 'Печать ценника';
                printButton.innerHTML = '&#129534;';
                printButton.onclick = function() {
                    openPrintModal(product);
                };
            }
            
            const articleContainer = document.createElement('div');
            articleContainer.style.display = 'flex';
            articleContainer.style.justifyContent = 'space-between';
            articleContainer.style.alignItems = 'center';
            articleContainer.style.marginBottom = '5px';
            
            articleContainer.appendChild(articleRow);
            articleContainer.appendChild(printButton);
            
            container.innerHTML = `
                <div class="product-field barcode">Штрихкод: ${highlightedBarcode}</div>
                <div class="product-field name">${highlightedName}</div>
                ${formatPriceWithDiscount(product)}
            `;
            
            container.insertBefore(articleContainer, container.firstChild);
            
            const stockInfo = document.createElement('div');
            stockInfo.innerHTML = `
                <div class="stock-info">
                    <div class="stock-title">Остатки:</div>
                    <div class="stock-item">
                        <span class="stock-name">ТОРГОВЫЙ ЗАЛ 10-11:</span>
                        <span class="stock-quantity ${product.stocks.warehouse1 < 0 ? 'negative' : 'positive'}">
                            ${formatNumber(product.stocks.warehouse1)} шт.
                        </span>
                    </div>
                    <div class="stock-item">
                        <span class="stock-name">СКЛАД кожгалантереи:</span>
                        <span class="stock-quantity ${product.stocks.warehouse2 < 0 ? 'negative' : 'positive'}">
                            ${formatNumber(product.stocks.warehouse2)} шт.
                        </span>
                    </div>
                </div>
            `;
            
            if (product.storageLocation && product.storageLocation.trim() !== '') {
                stockInfo.innerHTML += `
                    <div class="storage-location">
                        <div class="storage-title">Место хранения:</div>
                        <div class="storage-value">${product.storageLocation}</div>
                    </div>
                `;
            }
            
            productCard.appendChild(container);
            productCard.appendChild(stockInfo);
            
            return productCard;
        }

        function scrollToResults() {
            const resultsContainer = document.getElementById('resultsContainer');
            if (resultsContainer.style.display === 'block') {
                setTimeout(() => {
                    resultsContainer.scrollIntoView({
                        behavior: 'smooth',
                        block: 'start'
                    });
                }, 100);
            }
        }

        function displayResults(results, query, searchMode) {
            const resultsContainer = document.getElementById('resultsContainer');
            resultsContainer.innerHTML = '';

            if (results.length === 0) {
                resultsContainer.innerHTML = '<div class="no-results">Товары не найдены</div>';
                resultsContainer.style.display = 'block';
                scrollToResults();
                return;
            }

            const groupedResults = groupProductsByKey(results);
            const totalCount = results.length;
            const uniqueCount = groupedResults.length;

            const countElement = document.createElement('div');
            countElement.className = 'results-count';
            countElement.textContent = `Найдено товаров: ${totalCount} (${uniqueCount} уникальных)`;
            resultsContainer.appendChild(countElement);

            const modeElement = document.createElement('div');
            modeElement.className = 'search-mode';
            modeElement.textContent = `Режим поиска: ${searchMode}`;
            resultsContainer.appendChild(modeElement);

            groupedResults.forEach(product => {
                const productCard = createProductCard(product, query, searchMode);
                resultsContainer.appendChild(productCard);
            });

            resultsContainer.style.display = 'block';
            scrollToResults();
        }

        function searchProducts() {
            const searchMode = getCurrentSearchMode();
            
            let results = [];
            let query = '';
            let displaySearchMode = getSearchModeDisplayName(searchMode);

            if (searchMode === 'combined') {
                const articlePart = articleInput.value.trim();
                const namePart = nameInput.value.trim();
                const barcodePart = barcodeInput.value.trim();
                
                query = {
                    article: articlePart,
                    name: namePart,
                    barcode: barcodePart
                };
                
                results = performCombinedSearch(articlePart, namePart, barcodePart);
            } else {
                query = searchInput.value.trim();
                
                if (!query) {
                    resultsContainer.style.display = 'none';
                    return;
                }
                
                results = performSimpleSearch(query, searchMode);
            }

            displayResults(results, query, displaySearchMode);
        }

        function updateClearButton() {
            const mode = getCurrentSearchMode();
            let hasText = false;
            
            if (mode === 'combined') {
                hasText = articleInput.value.trim() !== '' || 
                          nameInput.value.trim() !== '' || 
                          barcodeInput.value.trim() !== '';
            } else {
                hasText = searchInput.value.trim() !== '';
            }
            
            if (hasText) {
                clearSearchBtn.style.display = 'block';
            } else {
                clearSearchBtn.style.display = 'none';
            }
        }

        function clearSearchFields() {
            const mode = getCurrentSearchMode();
            
            if (mode === 'combined') {
                articleInput.value = '';
                nameInput.value = '';
                barcodeInput.value = '';
            } else {
                searchInput.value = '';
                searchInput.focus();
            }
            
            updateClearButton();
            resultsContainer.style.display = 'none';
        }

        function showProductImage(product) {
            const modal = document.createElement('div');
            modal.className = 'modal-overlay';
            modal.id = 'imageModal';
            modal.style.display = 'flex';
            
            let imageCode = product.imageCode || '';
            let alternativeImageCode = product.alternativeImageCode || '';
            
            let imageUrl = '';
            
            if (imageCode) {
                const cleanCode = imageCode.trim();
                let fileName = cleanCode;
                if (!fileName.includes('.jpg') && !fileName.includes('.jpeg') && 
                    !fileName.includes('.png') && !fileName.includes('.gif')) {
                    fileName += '.jpg';
                }
                imageUrl = `https://pk.kubanstar.ru/images/virtuemart/product/${fileName}`;
            }
            
            modal.innerHTML = `
                <div class="modal-frame" style="max-width: 90%; max-height: 90%;">
                    <div style="text-align: center; padding: 20px;">
                        <h3 style="margin-bottom: 20px;">${product.article} - ${product.name}</h3>
                        <div style="max-height: 70vh; overflow: auto; margin: 20px 0;" id="imageContainer">
                            <img id="productImage" 
                                 src="${imageUrl || 'data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text x=%2250%25%22 y=%2250%25%22 font-size=%2250%22 text-anchor=%22middle%22 dy=%22.3em%22>&#128247;</text></svg>'}" 
                                 style="max-width: 100%; max-height: 60vh; border-radius: 8px; display: block; margin: 0 auto;"
                                 onerror="handleImageError(this, '${alternativeImageCode.replace(/'/g, "\\'")}')"
                                 alt="Изображение товара">
                            <div id="imageError" style="display: none; padding: 40px; color: #999;">
                                <div style="font-size: 48px; margin-bottom: 20px;">&#128247;</div>
                                <div style="font-size: 18px; font-weight: bold; color: #666;">Изображение не найдено</div>
                                <div style="font-size: 12px; margin-top: 10px; color: #999;">Пробуем альтернативный код...</div>
                            </div>
                        </div>
                        <div style="margin-top: 15px; text-align: center;">
                            <button onclick="this.closest('.modal-overlay').style.display='none'" 
                                    class="camera-btn" 
                                    style="background-color: #f44336; min-width: 200px;">
                                Закрыть
                            </button>
                        </div>
                    </div>
                </div>
            `;
            
            const oldModal = document.getElementById('imageModal');
            if (oldModal) {
                oldModal.remove();
            }
            
            document.body.appendChild(modal);
            
            modal.onclick = function(e) {
                if (e.target === modal) {
                    modal.style.display = 'none';
                }
            };
        }

        function handleImageError(imgElement, alternativeImageCode) {
            const errorDiv = document.getElementById('imageError');
            
            if (alternativeImageCode && alternativeImageCode.trim() !== '') {
                const cleanCode = alternativeImageCode.trim();
                let fileName = cleanCode;
                if (!fileName.includes('.jpg') && !fileName.includes('.jpeg') && 
                    !fileName.includes('.png') && !fileName.includes('.gif')) {
                    fileName += '.jpg';
                }
                const alternativeUrl = `https://pk.kubanstar.ru/images/virtuemart/product/${fileName}`;
                
                if (errorDiv) {
                    errorDiv.style.display = 'block';
                    errorDiv.innerHTML = `
                        <div style="font-size: 48px; margin-bottom: 20px;">&#128260;</div>
                        <div style="font-size: 18px; font-weight: bold; color: #666;">Загружаем альтернативное изображение...</div>
                        <div style="font-size: 12px; margin-top: 10px; color: #999;">Код: ${alternativeImageCode}</div>
                    `;
                }
                
                const newImg = new Image();
                newImg.onload = function() {
                    imgElement.src = alternativeUrl;
                    imgElement.style.display = 'block';
                    if (errorDiv) errorDiv.style.display = 'none';
                };
                newImg.onerror = function() {
                    if (errorDiv) {
                        errorDiv.style.display = 'block';
                        errorDiv.innerHTML = `
                            <div style="font-size: 48px; margin-bottom: 20px;">&#10060;</div>
                            <div style="font-size: 18px; font-weight: bold; color: #666;">Изображение не найдено</div>
                            <div style="font-size: 12px; margin-top: 10px; color: #999;">
                                Основной код: ${imgElement.src.includes('pk.kubanstar.ru') ? imgElement.src.split('/').pop() : 'не указан'}<br>
                                Альтернативный код: ${alternativeImageCode || 'не указан'}
                            </div>
                        `;
                    }
                    imgElement.style.display = 'none';
                };
                newImg.src = alternativeUrl;
            } else {
                if (errorDiv) {
                    errorDiv.style.display = 'block';
                    errorDiv.innerHTML = `
                        <div style="font-size: 48px; margin-bottom: 20px;">&#10060;</div>
                        <div style="font-size: 18px; font-weight: bold; color: #666;">Изображение не найдено</div>
                        <div style="font-size: 12px; margin-top: 10px; color: #999;">
                            Код изображения: ${imgElement.src.includes('pk.kubanstar.ru') ? imgElement.src.split('/').pop() : 'не указан'}
                        </div>
                    `;
                }
                imgElement.style.display = 'none';
            }
        }

     
        function initScrollToTopButton() {
            const scrollToTopBtn = document.getElementById('scrollToTopBtn');
            
            window.addEventListener('scroll', function() {
                if (window.pageYOffset > 300) {
                    scrollToTopBtn.classList.add('show');
                } else {
                    scrollToTopBtn.classList.remove('show');
                }
            });
            
            scrollToTopBtn.addEventListener('click', function() {
                window.scrollTo({
                    top: 0,
                    behavior: 'smooth'
                });
            });
        }

        // ===== ИНИЦИАЛИЗАЦИЯ =====
        
        const searchInput = document.getElementById('searchInput');
        const clearSearchBtn = document.getElementById('clearSearchBtn');
        const searchButton = document.getElementById('searchButton');
        const scanButtonAndroid = document.getElementById('scanButtonAndroid');
        const scanButtonIOS = document.getElementById('scanButtonIOS');
        const resultsContainer = document.getElementById('resultsContainer');
        const searchModeRadios = document.querySelectorAll('input[name="searchMode"]');
        const combinedSearchFields = document.getElementById('combinedSearchFields');
        const articleInput = document.getElementById('articleInput');
        const nameInput = document.getElementById('nameInput');
        const barcodeInput = document.getElementById('barcodeInput');
        
        const cameraModal = document.getElementById('cameraModal');
        const resultModal = document.getElementById('resultModal');
        const printModal = document.getElementById('printModal');
        const printModalIOS = document.getElementById('printModalIOS');
        const datesModal = document.getElementById('datesModal');		
        const closeCameraModal = document.getElementById('closeCameraModal');
        const closePrintModalBtn = document.getElementById('closePrintModal');
        const closePrintModalIOSBtn = document.getElementById('closePrintModalIOS');
        const closeDatesModalBtn = document.getElementById('closeDatesModal');
        
        const cameraVideo = document.getElementById('cameraVideo');
        const stopCameraBtn = document.getElementById('stopCamera');
        
        const resultCount = document.getElementById('resultCount');
        const resultProducts = document.getElementById('resultProducts');
        const continueScanBtn = document.getElementById('continueScanBtn');
        const closeResultBtn = document.getElementById('closeResultBtn');

        const printActionBtn = document.getElementById('printActionBtn');
        const printActionBtnIOS = document.getElementById('printActionBtnIOS');
        const scanPrintersBtnIOS = document.getElementById('scanPrintersBtnIOS');

        // Элементы iOS сканера
        const closeIOSScannerBtn = document.getElementById('closeIOSScanner');
        const switchIOSCameraBtn = document.getElementById('switchIOSCamera');

        function updateSearchUI() {
            const mode = getCurrentSearchMode();
            
            if (mode === 'combined') {
                combinedSearchFields.style.display = 'flex';
                searchInput.style.display = 'none';
            } else {
                combinedSearchFields.style.display = 'none';
                searchInput.style.display = 'block';
            }
            
            updateClearButton();
        }

        // ===== ОБРАБОТЧИКИ СОБЫТИЙ =====

        searchButton.addEventListener('click', searchProducts);

        clearSearchBtn.addEventListener('click', clearSearchFields);

        searchInput.addEventListener('input', updateClearButton);
        articleInput.addEventListener('input', updateClearButton);
        nameInput.addEventListener('input', updateClearButton);
        barcodeInput.addEventListener('input', updateClearButton);

        searchInput.addEventListener('keydown', function(e) {
            if (e.key === 'Enter') searchProducts();
        });

        articleInput.addEventListener('keydown', function(e) {
            if (e.key === 'Enter') searchProducts();
        });

        nameInput.addEventListener('keydown', function(e) {
            if (e.key === 'Enter') searchProducts();
        });

        barcodeInput.addEventListener('keydown', function(e) {
            if (e.key === 'Enter') searchProducts();
        });

        scanButtonAndroid.addEventListener('click', openCamera);
        scanButtonIOS.addEventListener('click', openIOSScanner);

        closeCameraModal.addEventListener('click', function() {
            stopCameraStream();
            cameraModal.style.display = 'none';
        });

        cameraModal.addEventListener('click', function(e) {
            if (e.target === cameraModal) {
                stopCameraStream();
                cameraModal.style.display = 'none';
            }
        });

        stopCameraBtn.addEventListener('click', function() {
            stopCameraStream();
            cameraModal.style.display = 'none';
        });

        continueScanBtn.addEventListener('click', function() {
            resultModal.style.display = 'none';
            setTimeout(() => {
                if (isIOS()) {
                    openIOSScanner();
                } else {
                    openCamera();
                }
            }, 300);
        });

        closeResultBtn.addEventListener('click', function() {
            resultModal.style.display = 'none';
        });

        resultModal.addEventListener('click', function(e) {
            if (e.target === resultModal) resultModal.style.display = 'none';
        });

        closePrintModalBtn.addEventListener('click', closePrintModal);
        closePrintModalIOSBtn.addEventListener('click', closePrintModalIOS);

        printModal.addEventListener('click', function(e) {
            if (e.target === printModal) closePrintModal();
        });

        printModalIOS.addEventListener('click', function(e) {
            if (e.target === printModalIOS) closePrintModalIOS();
        });

        closeDatesModalBtn.addEventListener('click', closeDatesModal);

        datesModal.addEventListener('click', function(e) {
            if (e.target === datesModal) closeDatesModal();
        });

        printActionBtn.addEventListener('click', handlePrint);
        printActionBtnIOS.addEventListener('click', handlePrintIOS);
        scanPrintersBtnIOS.addEventListener('click', scanForPrintersIOS);

        document.getElementById('current-date').addEventListener('click', openDatesModal);

        // Обработчики iOS сканера
        closeIOSScannerBtn.addEventListener('click', closeIOSScanner);
        
        switchIOSCameraBtn.addEventListener('click', switchIOSCamera);

        iosScannerModal.addEventListener('click', function(e) {
            if (e.target === iosScannerModal) closeIOSScanner();
        });

        searchModeRadios.forEach(radio => {
            radio.addEventListener('change', function() {
                updateSearchUI();
                if (searchInput.value.trim() || 
                    (getCurrentSearchMode() === 'combined' && 
                     (articleInput.value.trim() || nameInput.value.trim() || barcodeInput.value.trim()))) {
                    searchProducts();
                }
            });
        });

        window.addEventListener('load', function() {
            document.getElementById('modeArticle').checked = true;
            updateSearchUI();
            searchInput.focus();
            setupPlatformUI();
            initScrollToTopButton();

			if (DATA_UPDATE_DATE && DATA_UPDATE_DATE.trim() !== "") {
                let displayDate = DATA_UPDATE_DATE;
                if (DATA_UPDATE_DATE.includes(" ")) {
                    displayDate = DATA_UPDATE_DATE.split(" ")[0];
                }
                document.getElementById('current-date').textContent = displayDate;
            }
        });

        searchInput.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') clearSearchFields();
        });

        document.addEventListener('keydown', function(e) {
            if ((e.ctrlKey || e.metaKey) && e.key === 'f') {
                e.preventDefault();
                const mode = getCurrentSearchMode();
                if (mode === 'combined') {
                    articleInput.focus();
                } else {
                    searchInput.focus();
                    searchInput.select();
                }
            }
        });

        // Обработчик видимости страницы для iOS
        document.addEventListener('visibilitychange', function() {
            if (document.hidden && iosIsScanning) {
                closeIOSScanner();
            }
        });

        // Обработчик ориентации для iOS
        window.addEventListener('orientationchange', function() {
            if (iosIsScanning) {
                setTimeout(() => {
                    if (iosIsScanning) {
                        closeIOSScanner();
                        setTimeout(openIOSScanner, 500);
                    }
                }, 300);
            }
        });

        // Инициализация сохраненного принтера для iOS
        if (isIOS()) {
            savedBluetoothDeviceId = localStorage.getItem('savedPrinterId');
        }
    </script>
</body>
</html>