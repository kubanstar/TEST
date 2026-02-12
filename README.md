<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>Поиск товаров</title>
    <script src="https://unpkg.com/html5-qrcode"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
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

        .print-button:active {
            background-color: #e0e0e0;
            transform: scale(0.95);
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
            backdrop-filter: blur(5px);
        }
        
        .print-modal-content-new {
            background: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            max-width: 450px;
            width: 90%;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            max-height: 90vh;
            overflow-y: auto;
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
            white-space: pre-line;
            text-align: left;
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
            backdrop-filter: blur(5px);
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
        
        .ble-printer-list {
            max-height: 300px;
            overflow-y: auto;
            margin: 15px 0;
            border: 1px solid #ddd;
            border-radius: 8px;
            background: white;
        }
        
        .ble-printer-item {
            padding: 15px;
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
            font-size: 16px;
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
            padding: 8px 16px;
            border-radius: 6px;
            font-size: 14px;
            font-weight: 600;
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
            border-left: 4px solid #4CAF50;
        }
        
        .ble-no-devices {
            padding: 20px;
            text-align: center;
            color: #666;
            font-style: italic;
        }
        
        .connected-printer-info {
            margin-bottom: 15px;
            padding: 15px;
            background-color: #e8f5e9;
            border-radius: 8px;
            border-left: 4px solid #4CAF50;
            text-align: left;
        }
        
        .connected-printer-title {
            font-weight: bold;
            color: #2e7d32;
            margin-bottom: 5px;
            font-size: 14px;
        }
        
        .connected-printer-name {
            font-size: 16px;
            font-weight: 600;
            color: #333;
        }
        
        .bluetooth-hint {
            background-color: #e3f2fd;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 15px;
            font-size: 14px;
            color: #0c5460;
            text-align: left;
            border-left: 4px solid #2196F3;
        }
        
        .bluetooth-hint i {
            font-style: normal;
            font-weight: bold;
        }
        
        @media (prefers-color-scheme: dark) {
            body {
                background-color: #1a1a1a;
            }
            
            .search-container {
                background-color: #2d2d2d;
                color: #fff;
            }
            
            .product-card {
                background-color: #333;
                color: #fff;
            }
            
            .modal-frame {
                background-color: #2d2d2d;
                color: #fff;
            }
            
            .print-modal-content-new {
                background-color: #2d2d2d;
                color: #fff;
            }
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
            <button class="clear-search-btn" id="clearSearchBtn" title="Очистить поле поиска">✕</button>
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
                <span class="scan-icon">📷</span> Сканировать штрихкод
            </button>
            <button class="scan-button scan-button-ios" id="scanButtonIOS" style="display: none;">
                <span class="scan-icon">📷</span> Сканировать штрихкод
            </button>
        </div>

        <div class="barcode-supported">
            <span class="barcode-format" id="current-date"></span>
        </div>

        <div id="printStatus" class="print-status"></div>
        
        <div class="results-container" id="resultsContainer"></div>
    </div>

    <button class="scroll-to-top-btn" id="scrollToTopBtn" title="Наверх">▲</button>

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
                <div class="ios-loader" id="iosScannerLoader"></div>
                
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
                    🔄 Переключить камеру
                </button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="resultModal">
        <div class="modal-frame scan-result-frame">
            <div class="scan-result-products" id="resultProducts"></div>
            <div class="scan-result-count" id="resultCount"></div>
            <div class="scan-result-actions">
                <button class="action-btn continue-scan-btn" id="continueScanBtn">
                    📷 Сканировать еще
                </button>
                <button class="action-btn close-result-btn" id="closeResultBtn">
                    Закрыть
                </button>
            </div>
        </div>
    </div>

    <div class="print-modal-new" id="printModal">
        <div class="print-modal-content-new">
            <h3>Печать ценника (Android)</h3>
            
            <div id="printerStatus" class="printer-status printer-connecting">
                Подключаюсь к принтеру...
            </div>

            <div class="price-tag-preview">
                <canvas id="priceTagPreviewCanvas" class="price-tag-canvas" width="440" height="284"></canvas>
            </div>
            
            <button class="print-action-btn" id="printActionBtn" disabled>
                🖨️ Распечатать
            </button>
            
            <button class="close-modal" id="closePrintModal" style="margin-top: 15px;">
                Закрыть
            </button>
        </div>
    </div>

    <div class="print-modal-new" id="printModalIOS">
        <div class="print-modal-content-new">
            <h3>Печать ценника (iOS)</h3>
            
            <div id="printerStatusIOS" class="printer-status printer-disconnected">
                ❌ Принтер не подключен
            </div>

            <div class="bluetooth-hint">
                <strong>🔵 Bluetooth</strong><br>
                Убедитесь, что принтер включен и находится рядом.
                Нажмите "Найти принтер" и выберите Xprinter из списка.
            </div>

            <div id="connectedPrinterInfoIOS" class="connected-printer-info" style="display: none;">
                <div class="connected-printer-title">✅ Подключено:</div>
                <div id="connectedPrinterNameIOS" class="connected-printer-name">Xprinter XP-P323B</div>
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

    <div class="modal-overlay" id="datesModal">
        <div class="modal-frame dates-modal">
            <div class="dates-header">
                <div class="current-date-display" id="modalCurrentDate">Дата обновления: 04.02.2026</div>
                <div class="data-update-container" id="dataUpdateContainer">Данные на : 14:07</div>
            </div>
            <div id="datesContent" class="dates-content"></div>
            <button class="close-modal" id="closeDatesModal" style="margin-top: 15px;">
                Закрыть
            </button>
        </div>
    </div>

    <script>
        // ===== КОНФИГУРАЦИЯ =====
        const SHOW_WHOLESALE_PLUS = false;
        const DATA_UPDATE_DATE = "";
        const URAL_OFFICE_DATE = "";
        const URAL_DATE = "";

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
        
        // Переменные для iOS сканера
        let iosHtml5QrCode = null;
        let iosIsScanning = false;
        let iosLastScannedCode = '';
        let iosCurrentFacingMode = 'environment';
        
        // UUID для Bluetooth принтера
        const PRINTER_SERVICE_UUIDS = [
            '0000ff00-0000-1000-8000-00805f9b34fb',
            '000018f0-0000-1000-8000-00805f9b34fb',
            '6e400001-b5a3-f393-e0a9-e50e24dcca9e',
            '0000ae30-0000-1000-8000-00805f9b34fb',
            '49535343-fe7d-4ae5-8fa9-9fafd205e455'
        ];

        const PRINTER_CHARACTERISTIC_UUIDS = [
            '0000ff01-0000-1000-8000-00805f9b34fb',
            '0000ff02-0000-1000-8000-00805f9b34fb',
            '6e400002-b5a3-f393-e0a9-e50e24dcca9e',
            '6e400003-b5a3-f393-e0a9-e50e24dcca9e',
            '49535343-8841-43f4-a8d4-ecbe34729bb3',
            '0000ae01-0000-1000-8000-00805f9b34fb'
        ];

        // Пример данных
        const productsData = `2002000149572;620-107K;Портмоне + зажим "SOMUCH" мат, цв: черный;570,00;750,00;587,00;;;;;;;;Sk000009622_1;
2002000149589;411;Сумочка на пояс GOLD CORAL кожа, цв.: черный;800,00;1050,00;824,00;;;;;;;;Sk000009623_1;
2002000149596;BL-S018-1;Портмоне+зажим "HETINO" глянец, тонкое, цв: черный;630,00;850,00;649,00;;;;;;;;Sk000009624_1;
2002000149602;ST6749-3;Сумка мужская POLO А5+, цвет: чёрный;940,00;1250,00;968,00;;;;;;;;Sk000009625_1;
2002000149626;S1394-5;Сумка мужская SOMUCH А4+, вертикальная, цвет: чёрный;1600,00;2400,00;1648,00;6;;11-1/B-5.2;;;;;Sk000009627_1;`;

        // ===== ВСПОМОГАТЕЛЬНЫЕ ФУНКЦИИ =====
        function isIOS() {
            return /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;
        }

        function isAndroid() {
            return /Android/.test(navigator.userAgent);
        }

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

        function groupProductsByKey(products) {
            const groups = {};
            
            products.forEach(product => {
                const key = `${product.article}|${product.name}|${product.wholesalePrice}|${product.retailPrice}|${product.wholesalePlusPrice}|${product.stocks.warehouse1}|${product.stocks.warehouse2}|${product.discountPercent}|${product.discountPriceOpt}|${product.discountPriceRetail}|${product.storageLocation}|${product.imageCode}|${product.alternativeImageCode}`;
                
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

        // ===== ФУНКЦИИ ДЛЯ СОЗДАНИЯ ЦЕННИКА =====
        function createPriceTagImage(product) {
            const canvas = document.createElement('canvas');
            canvas.width = 440;
            canvas.height = 240;
            const ctx = canvas.getContext('2d');

            ctx.fillStyle = 'white';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = 'black';
            ctx.textAlign = 'center';

            const fonts = {
                article: 25,
                product: 19,
                price: 22,
                price2: 21,
                date: 16,
                company: 18
            };

            ctx.font = `bold ${fonts.article}px "Arial"`;
            ctx.fillText(product.article, canvas.width / 2, 35);

            ctx.beginPath();
            ctx.moveTo(10, 45);
            ctx.lineTo(canvas.width - 10, 45);
            ctx.lineWidth = 3;
            ctx.stroke();

            ctx.font = `${fonts.product}px "Arial"`;
            
            let lines = [];
            if (product.name.length <= 35) {
                lines.push(product.name);
            } else {
                let possibleTwoLines = splitIntoTwoLines(product.name, 35);
                if (possibleTwoLines && possibleTwoLines.length === 2) {
                    lines = possibleTwoLines;
                } else {
                    lines = splitIntoThreeLines(product.name, 35);
                }
            }

            let yPos = 70;
            lines.forEach(line => {
                ctx.fillText(line.trim(), canvas.width / 2, yPos);
                yPos += 25;
            });

            ctx.beginPath();
            ctx.moveTo(10, yPos - 5);
            ctx.lineTo(canvas.width - 10, yPos - 5);
            ctx.lineWidth = 3;
            ctx.stroke();

            ctx.font = `bold ${fonts.price}px "Arial"`;
            const retailPriceFormatted = formatNumber(product.retailPrice, true);
            ctx.fillText(`РОЗ: ${retailPriceFormatted} Руб.`, canvas.width / 2, 135);

            ctx.beginPath();
            ctx.moveTo(10, 150);
            ctx.lineTo(canvas.width - 10, 150);
            ctx.lineWidth = 3;
            ctx.stroke();

            ctx.font = `${fonts.price2}px "Arial"`;
            const wholesalePriceNum = parseFloatValue(product.wholesalePrice);
            const wholesalePriceFormatted = Math.round(wholesalePriceNum).toString();
            const wholesaleCode = wholesalePriceFormatted.padStart(6, '0');
            ctx.fillText(`АРТ000${wholesaleCode}`, canvas.width / 2, 180);

            ctx.beginPath();
            ctx.moveTo(10, 190);
            ctx.lineTo(canvas.width - 10, 190);
            ctx.lineWidth = 3;
            ctx.stroke();

            const today = new Date();
            const dateStr = `${today.getDate().toString().padStart(2, '0')}.${(today.getMonth()+1).toString().padStart(2, '0')}.${today.getFullYear()}`;
            ctx.font = `bold ${fonts.date}px "Arial"`;
            ctx.fillText(dateStr, canvas.width / 2, 215);

            ctx.beginPath();
            ctx.moveTo(10, 225);
            ctx.lineTo(canvas.width - 10, 225);
            ctx.lineWidth = 3;
            ctx.stroke();

            ctx.font = `${fonts.company}px "Arial"`;
            ctx.fillText('ИП Мааруф Р.', canvas.width / 2, 240);
            
            return canvas;
        }

        function splitIntoTwoLines(text, maxChars) {
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
                    if (currentLine) lines.push(currentLine);
                    currentLine = word;
                    
                    if (lines.length === 2) {
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
                    
                    if ((r + g + b) < 384) {
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
            const bytesPerLine = bitmap.bytesPerLine;
            const height = bitmap.height;
            
            const command = new Uint8Array(bitmap.data.length + 8);
            
            command[0] = 0x1D;
            command[1] = 0x76;
            command[2] = 0x30;
            command[3] = 0x00;
            command[4] = bytesPerLine & 0xFF;
            command[5] = (bytesPerLine >> 8) & 0xFF;
            command[6] = height & 0xFF;
            command[7] = (height >> 8) & 0xFF;
            
            command.set(bitmap.data, 8);
            
            return command;
        }

        // ===== ФУНКЦИИ ДЛЯ ANDROID (WEB SERIAL) =====
        function updatePrinterStatus(message, type = 'connecting') {
            const statusEl = document.getElementById('printerStatus');
            if (!statusEl) return;
            
            statusEl.classList.remove('printer-connected', 'printer-disconnected', 'printer-connecting');
            
            switch(type) {
                case 'connected':
                    statusEl.classList.add('printer-connected');
                    statusEl.innerHTML = '✅ ' + message;
                    break;
                case 'disconnected':
                    statusEl.classList.add('printer-disconnected');
                    statusEl.innerHTML = '❌ ' + message;
                    break;
                case 'connecting':
                    statusEl.classList.add('printer-connecting');
                    statusEl.innerHTML = '⏳ ' + message;
                    break;
                default:
                    statusEl.textContent = message;
            }
        }

        async function connectToPrinter() {
            try {
                updatePrinterStatus('Подключаюсь к принтеру...', 'connecting');
                
                if (!navigator.serial) {
                    throw new Error('Web Serial не поддерживается');
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
                console.error('Ошибка подключения:', error);
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
                throw new Error('Принтер не подключен');
            }
            await serialWriter.write(data);
            return true;
        }

        async function printPriceTag(product) {
            try {
                if (!isPrinterConnected) {
                    const connected = await connectToPrinter();
                    if (!connected) throw new Error('Не удалось подключиться к принтеру');
                }
                
                const canvas = createPriceTagImage(product);
                const bitmap = canvasToEscPosBitmap(canvas);
                const imageCommand = createEscPosImageCommand(bitmap);
                
                const fullCommand = new Uint8Array(imageCommand.length + 10);
                fullCommand[0] = 0x1B;
                fullCommand[1] = 0x40;
                fullCommand.set(imageCommand, 2);
                fullCommand[imageCommand.length + 2] = 0x0A;
                fullCommand[imageCommand.length + 3] = 0x0A;
                
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
            
            const previewCanvas = createPriceTagImage(product);
            ctx.drawImage(previewCanvas, 0, 0, canvas.width, canvas.height);
        }

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
            printBtn.textContent = 'Подключаюсь...';
            
            try {
                const connected = await connectToPrinter();
                if (connected) {
                    printBtn.disabled = false;
                    printBtn.textContent = '🖨️ Распечатать';
                } else {
                    printBtn.disabled = true;
                    printBtn.textContent = '❌ Ошибка подключения';
                }
            } catch (error) {
                printBtn.disabled = true;
                printBtn.textContent = '❌ Ошибка';
            }
        }

        function closePrintModal() {
            document.getElementById('printModal').style.display = 'none';
            setTimeout(() => disconnectFromPrinter(), 1000);
        }

        async function handlePrint() {
            if (!currentProductForPrint) return;
            
            const printBtn = document.getElementById('printActionBtn');
            printBtn.disabled = true;
            printBtn.textContent = 'Печатаю...';
            
            try {
                await printPriceTag(currentProductForPrint);
                showPrintStatus('✅ Ценник отправлен на печать!', 'success');
                setTimeout(closePrintModal, 1500);
            } catch (error) {
                showPrintStatus('❌ Ошибка: ' + error.message, 'error');
                printBtn.disabled = false;
                printBtn.textContent = '🖨️ Распечатать';
            }
        }

        // ===== ФУНКЦИИ ДЛЯ iOS (WEB BLUETOOTH) =====
        function updatePrinterStatusIOS(message, type = 'disconnected') {
            const statusEl = document.getElementById('printerStatusIOS');
            if (!statusEl) return;
            
            statusEl.classList.remove('printer-connected', 'printer-disconnected', 'printer-connecting');
            
            switch(type) {
                case 'connected':
                    statusEl.classList.add('printer-connected');
                    statusEl.innerHTML = '✅ ' + message;
                    break;
                case 'disconnected':
                    statusEl.classList.add('printer-disconnected');
                    statusEl.innerHTML = '❌ ' + message;
                    break;
                case 'connecting':
                    statusEl.classList.add('printer-connecting');
                    statusEl.innerHTML = '⏳ ' + message;
                    break;
                default:
                    statusEl.textContent = message;
            }
        }

        async function scanForBLEPrinters() {
            try {
                updatePrinterStatusIOS('Поиск принтеров...', 'connecting');
                
                if (!navigator.bluetooth) {
                    throw new Error('Web Bluetooth не поддерживается.\nИспользуйте Safari с HTTPS');
                }

                const options = {
                    acceptAllDevices: true,
                    optionalServices: PRINTER_SERVICE_UUIDS
                };

                console.log('Запрос Bluetooth устройств...');
                const device = await navigator.bluetooth.requestDevice(options);
                
                if (!device) {
                    throw new Error('Принтер не выбран');
                }

                console.log('Выбрано устройство:', device.name || 'Xprinter');
                bluetoothDevice = device;
                
                try {
                    localStorage.setItem('savedPrinterId', device.id);
                    localStorage.setItem('savedPrinterName', device.name || 'Xprinter');
                } catch (e) {
                    console.warn('Не удалось сохранить в localStorage');
                }

                await connectToBLEPrinter(device);
                return true;

            } catch (error) {
                console.error('Ошибка поиска:', error);
                
                let errorMessage = 'Ошибка: ';
                if (error.message.includes('User cancelled')) {
                    errorMessage = 'Поиск отменен. Нажмите "Найти принтер" еще раз.';
                } else if (error.message.includes('Bluetooth adapter')) {
                    errorMessage = 'Bluetooth не доступен. Включите Bluetooth в настройках.';
                } else {
                    errorMessage += error.message;
                }
                
                updatePrinterStatusIOS(errorMessage, 'disconnected');
                return false;
            }
        }

        async function connectToBLEPrinter(device) {
            try {
                updatePrinterStatusIOS('Подключение к принтеру...', 'connecting');
                
                device.addEventListener('gattserverdisconnected', onDeviceDisconnected);
                
                const server = await device.gatt.connect();
                console.log('GATT сервер подключен');
                
                let characteristic = null;
                let foundService = null;

                for (const serviceUUID of PRINTER_SERVICE_UUIDS) {
                    try {
                        console.log('Пробуем сервис:', serviceUUID);
                        foundService = await server.getPrimaryService(serviceUUID);
                        console.log('Найден сервис:', serviceUUID);
                        break;
                    } catch (e) {
                        console.log('Сервис не найден:', serviceUUID);
                    }
                }

                if (!foundService) {
                    throw new Error('Не найден сервис принтера');
                }

                for (const charUUID of PRINTER_CHARACTERISTIC_UUIDS) {
                    try {
                        console.log('Пробуем характеристику:', charUUID);
                        characteristic = await foundService.getCharacteristic(charUUID);
                        console.log('Найдена характеристика:', charUUID);
                        break;
                    } catch (e) {
                        console.log('Характеристика не найдена:', charUUID);
                    }
                }

                if (!characteristic) {
                    throw new Error('Не найдена характеристика для печати');
                }

                bluetoothCharacteristic = characteristic;
                isBLEPrinterConnected = true;
                
                updatePrinterStatusIOS('Принтер подключен', 'connected');
                
                const connectedInfo = document.getElementById('connectedPrinterInfoIOS');
                if (connectedInfo) {
                    connectedInfo.style.display = 'block';
                    const nameEl = document.getElementById('connectedPrinterNameIOS');
                    if (nameEl) nameEl.textContent = device.name || 'Xprinter XP-P323B';
                }
                
                const printBtn = document.getElementById('printActionBtnIOS');
                if (printBtn) printBtn.disabled = false;
                
                return true;

            } catch (error) {
                console.error('Ошибка подключения:', error);
                updatePrinterStatusIOS(`Ошибка: ${error.message}`, 'disconnected');
                return false;
            }
        }

        function onDeviceDisconnected(event) {
            console.log('Принтер отключился');
            isBLEPrinterConnected = false;
            bluetoothCharacteristic = null;
            
            updatePrinterStatusIOS('Принтер отключен', 'disconnected');
            
            const connectedInfo = document.getElementById('connectedPrinterInfoIOS');
            if (connectedInfo) connectedInfo.style.display = 'none';
            
            const printBtn = document.getElementById('printActionBtnIOS');
            if (printBtn) printBtn.disabled = true;
        }

        async function disconnectFromBLEPrinter() {
            try {
                if (bluetoothDevice && bluetoothDevice.gatt.connected) {
                    bluetoothDevice.gatt.disconnect();
                }
                bluetoothCharacteristic = null;
                isBLEPrinterConnected = false;
                
                updatePrinterStatusIOS('Принтер отключен', 'disconnected');
                
                const connectedInfo = document.getElementById('connectedPrinterInfoIOS');
                if (connectedInfo) connectedInfo.style.display = 'none';
                
                const printBtn = document.getElementById('printActionBtnIOS');
                if (printBtn) printBtn.disabled = true;
                
                return true;
            } catch (error) {
                console.error('Ошибка отключения:', error);
                return false;
            }
        }

        async function sendBLEData(data) {
            if (!isBLEPrinterConnected || !bluetoothCharacteristic) {
                throw new Error('Принтер не подключен');
            }
            
            try {
                await bluetoothCharacteristic.writeValue(data);
                return true;
            } catch (error) {
                console.error('Ошибка отправки BLE:', error);
                throw error;
            }
        }

        async function printPriceTagIOS(product) {
            try {
                if (!isBLEPrinterConnected) {
                    if (savedBluetoothDeviceId) {
                        try {
                            updatePrinterStatusIOS('Восстановление подключения...', 'connecting');
                            const devices = await navigator.bluetooth.getDevices();
                            const savedDevice = devices.find(d => d.id === savedBluetoothDeviceId);
                            if (savedDevice) {
                                bluetoothDevice = savedDevice;
                                await connectToBLEPrinter(savedDevice);
                            } else {
                                throw new Error('Принтер не найден');
                            }
                        } catch (e) {
                            throw new Error('Принтер не подключен. Нажмите "Найти принтер"');
                        }
                    } else {
                        throw new Error('Принтер не подключен. Нажмите "Найти принтер"');
                    }
                }
                
                const canvas = createPriceTagImage(product);
                const bitmap = canvasToEscPosBitmap(canvas);
                const imageCommand = createEscPosImageCommand(bitmap);
                
                const fullCommand = new Uint8Array(imageCommand.length + 10);
                fullCommand[0] = 0x1B;
                fullCommand[1] = 0x40;
                fullCommand.set(imageCommand, 2);
                fullCommand[imageCommand.length + 2] = 0x0A;
                fullCommand[imageCommand.length + 3] = 0x0A;
                
                await sendBLEData(fullCommand);
                return true;
                
            } catch (error) {
                console.error('Ошибка печати iOS:', error);
                throw error;
            }
        }

        function updatePriceTagPreviewIOS(product) {
            const canvas = document.getElementById('priceTagPreviewCanvasIOS');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = 'white';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            const previewCanvas = createPriceTagImage(product);
            ctx.drawImage(previewCanvas, 0, 0, canvas.width, canvas.height);
        }

        async function openPrintModalIOS(product) {
            currentProductForPrintIOS = product;
            const modal = document.getElementById('printModalIOS');
            if (!modal) return;
            
            modal.style.display = 'flex';
            updatePriceTagPreviewIOS(product);
            
            const printBtn = document.getElementById('printActionBtnIOS');
            const statusEl = document.getElementById('printerStatusIOS');
            
            printBtn.disabled = true;
            
            if (!navigator.bluetooth) {
                updatePrinterStatusIOS('❌ Web Bluetooth не поддерживается.\n\nИспользуйте Safari с HTTPS', 'disconnected');
                return;
            }
            
            updatePrinterStatusIOS('Принтер не подключен. Нажмите "Найти принтер"', 'disconnected');
            
            try {
                savedBluetoothDeviceId = localStorage.getItem('savedPrinterId');
                if (savedBluetoothDeviceId && !isBLEPrinterConnected) {
                    updatePrinterStatusIOS('Попытка восстановить подключение...', 'connecting');
                    const devices = await navigator.bluetooth.getDevices();
                    const savedDevice = devices.find(d => d.id === savedBluetoothDeviceId);
                    if (savedDevice) {
                        bluetoothDevice = savedDevice;
                        await connectToBLEPrinter(savedDevice);
                    }
                }
            } catch (e) {
                console.warn('Не удалось восстановить подключение:', e);
            }
            
            if (isBLEPrinterConnected) {
                printBtn.disabled = false;
            }
        }

        function closePrintModalIOS() {
            document.getElementById('printModalIOS').style.display = 'none';
            setTimeout(() => disconnectFromBLEPrinter(), 1000);
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
                showPrintStatus('✅ Ценник отправлен на печать!', 'success');
                setTimeout(() => closePrintModalIOS(), 1500);
            } catch (error) {
                showPrintStatus('❌ Ошибка: ' + error.message, 'error');
                printBtn.disabled = false;
                printBtn.textContent = '🖨️ Распечатать';
            }
        }

        // ===== ФУНКЦИИ ДЛЯ СКАНИРОВАНИЯ =====
        async function initBarcodeDetector() {
            if (!('BarcodeDetector' in window)) return null;
            
            try {
                const formats = await BarcodeDetector.getSupportedFormats();
                const supportedFormats = formats.filter(format => 
                    ['ean_13', 'ean_8', 'upc_a', 'upc_e', 'code_39', 'code_128', 'codabar'].includes(format)
                );
                
                if (supportedFormats.length === 0) return null;
                return new BarcodeDetector({ formats: supportedFormats });
            } catch (error) {
                console.error('Ошибка инициализации BarcodeDetector:', error);
                return null;
            }
        }

        function setupPlatformUI() {
            const scanButtonAndroid = document.getElementById('scanButtonAndroid');
            const scanButtonIOS = document.getElementById('scanButtonIOS');
            
            if (isIOS()) {
                scanButtonAndroid.style.display = 'none';
                scanButtonIOS.style.display = 'flex';
                document.getElementById('searchButton').style.maxWidth = '300px';
            } else {
                scanButtonAndroid.style.display = 'flex';
                scanButtonIOS.style.display = 'none';
            }
        }

        async function openCamera() {
            try {
                stopCameraStream();
                
                const constraints = {
                    video: { facingMode: 'environment' },
                    audio: false
                };
                
                stream = await navigator.mediaDevices.getUserMedia(constraints);
                const video = document.getElementById('cameraVideo');
                video.srcObject = stream;
                document.getElementById('cameraModal').style.display = 'flex';
                await video.play();
                
                if (!barcodeDetector) {
                    barcodeDetector = await initBarcodeDetector();
                }
                
                if (barcodeDetector) {
                    startBarcodeDetection(barcodeDetector);
                }
                
            } catch (error) {
                console.error('Ошибка доступа к камере:', error);
                alert('Не удалось получить доступ к камере');
            }
        }

        function startBarcodeDetection(detector) {
            const video = document.getElementById('cameraVideo');
            const canvas = document.createElement('canvas');
            const context = canvas.getContext('2d');
            
            scanInterval = setInterval(async () => {
                if (video.readyState === video.HAVE_ENOUGH_DATA) {
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);
                    
                    try {
                        const barcodes = await detector.detect(canvas);
                        if (barcodes && barcodes.length > 0) {
                            handleScannedCode(barcodes[0].rawValue);
                        }
                    } catch (error) {
                        console.error('Ошибка детектирования:', error);
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
            const video = document.getElementById('cameraVideo');
            if (video) video.srcObject = null;
        }

        function handleScannedCode(code) {
            if (!code) return;
            
            stopCameraStream();
            document.getElementById('cameraModal').style.display = 'none';
            document.getElementById('modeBarcode').checked = true;
            updateSearchUI();
            
            const cleanCode = code.toString().trim();
            document.getElementById('searchInput').value = cleanCode;
            updateClearButton();
            
            const results = performSimpleSearch(cleanCode, 'barcode');
            showScanResults(cleanCode, results);
        }

        // ===== ФУНКЦИИ ДЛЯ iOS СКАНЕРА =====
        async function openIOSScanner() {
            const modal = document.getElementById('iosScannerModal');
            modal.style.display = 'block';
            
            document.getElementById('iosScannerLoader').style.display = 'block';
            showIOSScannerStatus('Инициализация камеры...');
            
            setTimeout(() => initIOSBarcodeScanner(), 300);
        }

        function initIOSBarcodeScanner() {
            try {
                if (iosHtml5QrCode && iosIsScanning) {
                    iosHtml5QrCode.stop().then(() => {
                        iosHtml5QrCode.clear();
                        iosHtml5QrCode = null;
                    }).catch(() => {});
                }

                const config = {
                    fps: 10,
                    qrbox: { width: 250, height: 150 },
                    rememberLastUsedCamera: true,
                    supportedScanTypes: [Html5QrcodeScanType.SCAN_TYPE_CAMERA],
                    videoConstraints: {
                        facingMode: { ideal: "environment" }
                    }
                };

                iosHtml5QrCode = new Html5Qrcode("ios-qr-reader");

                iosHtml5QrCode.start(
                    { facingMode: "environment" },
                    config,
                    onIOSScanSuccess,
                    onIOSScanError
                ).then(() => {
                    iosIsScanning = true;
                    document.getElementById('iosScannerLoader').style.display = 'none';
                    document.getElementById('iosNoCameraMessage').style.display = 'none';
                    hideIOSScannerStatus();
                }).catch(err => {
                    console.error('Ошибка запуска iOS сканера:', err);
                    showIOSNoCameraMessage();
                });

            } catch (error) {
                console.error('Критическая ошибка:', error);
                showIOSNoCameraMessage();
            }
        }

        function onIOSScanSuccess(decodedText) {
            if (iosLastScannedCode === decodedText) return;
            iosLastScannedCode = decodedText;
            
            if (iosHtml5QrCode && iosIsScanning) {
                iosHtml5QrCode.stop().then(() => iosIsScanning = false).catch(() => {});
            }
            
            setTimeout(() => {           
                closeIOSScanner();
                
                document.getElementById('modeBarcode').checked = true;
                updateSearchUI();
                
                const cleanCode = decodedText.toString().trim();
                document.getElementById('searchInput').value = cleanCode;
                updateClearButton();
                
                const results = performSimpleSearch(cleanCode, 'barcode');
                showScanResults(cleanCode, results);
                
                setTimeout(() => iosLastScannedCode = '', 3000);
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
            if (iosHtml5QrCode && iosIsScanning) {
                iosHtml5QrCode.stop().then(() => {
                    iosHtml5QrCode.clear();
                    iosHtml5QrCode = null;
                    iosIsScanning = false;
                }).catch(() => {});
            }
            
            document.getElementById('iosScannerModal').style.display = 'none';
            document.getElementById('iosNoCameraMessage').style.display = 'none';
            hideIOSScannerStatus();
        }

        function showIOSScannerStatus(message) {
            const status = document.getElementById('iosScannerStatus');
            if (status) {
                status.textContent = message;
                status.style.display = 'block';
            }
        }

        function hideIOSScannerStatus() {
            const status = document.getElementById('iosScannerStatus');
            if (status) status.style.display = 'none';
        }

        // ===== ФУНКЦИИ ПОИСКА =====
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
                    if (product.article.toLowerCase().includes(articlePart.toLowerCase())) matches++;
                }
                
                if (namePart && namePart.trim() !== '') {
                    totalConditions++;
                    if (product.name.toLowerCase().includes(namePart.toLowerCase())) matches++;
                }
                
                if (barcodePart && barcodePart.trim() !== '') {
                    totalConditions++;
                    if (product.barcode.includes(barcodePart)) matches++;
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
            
            const hasImage = product.imageCode && product.imageCode.trim() !== '';
            
            const articleContainer = document.createElement('div');
            articleContainer.style.display = 'flex';
            articleContainer.style.justifyContent = 'space-between';
            articleContainer.style.alignItems = 'center';
            articleContainer.style.marginBottom = '5px';
            
            const articleRow = document.createElement('div');
            articleRow.className = 'article';
            articleRow.innerHTML = `Артикул: ${product.article}`;
            
            if (hasImage) {
                const imageBtn = document.createElement('button');
                imageBtn.className = 'image-button';
                imageBtn.innerHTML = '🖼️';
                imageBtn.onclick = () => showProductImage(product);
                articleRow.appendChild(imageBtn);
            }
            
            const printBtn = document.createElement('button');
            printBtn.className = 'print-button';
            printBtn.innerHTML = '🖨️';
            
            if (isIOS()) {
                printBtn.onclick = () => openPrintModalIOS(product);
                printBtn.title = 'Печать ценника (iOS)';
            } else {
                printBtn.onclick = () => openPrintModal(product);
                printBtn.title = 'Печать ценника';
            }
            
            articleContainer.appendChild(articleRow);
            articleContainer.appendChild(printBtn);
            
            const container = document.createElement('div');
            container.innerHTML = `
                <div class="product-field barcode">Штрихкод: ${product.barcode}</div>
                <div class="product-field name">${product.name}</div>
                ${formatPriceWithDiscount(product)}
            `;
            
            container.insertBefore(articleContainer, container.firstChild);
            productCard.appendChild(container);
            productCard.appendChild(createStockInfo(product));
            
            return productCard;
        }

        function createStockInfo(product) {
            const div = document.createElement('div');
            div.innerHTML = `
                <div class="stock-info">
                    <div class="stock-title">Остатки:</div>
                    <div class="stock-item">
                        <span class="stock-name">ТОРГОВЫЙ ЗАЛ:</span>
                        <span class="stock-quantity ${product.stocks.warehouse1 < 0 ? 'negative' : 'positive'}">
                            ${formatNumber(product.stocks.warehouse1)} шт.
                        </span>
                    </div>
                    <div class="stock-item">
                        <span class="stock-name">СКЛАД:</span>
                        <span class="stock-quantity ${product.stocks.warehouse2 < 0 ? 'negative' : 'positive'}">
                            ${formatNumber(product.stocks.warehouse2)} шт.
                        </span>
                    </div>
                </div>
                ${product.storageLocation ? `
                    <div class="storage-location">
                        <div class="storage-title">Место хранения:</div>
                        <div class="storage-value">${product.storageLocation}</div>
                    </div>
                ` : ''}
            `;
            return div;
        }

        function formatPriceWithDiscount(product) {
            const hasDiscount = product.discountPercent && product.discountPercent.trim() !== '';
            
            let html = '<div class="price-container">';
            
            if (hasDiscount) {
                html += `
                    <div class="price-line">
                        <span class="price-label">Оптовая:</span>
                        <span class="discount-price">${product.discountPriceOpt || product.wholesalePrice} руб.</span>
                    </div>
                    <div class="old-price-container">
                        <span class="original-price">${product.wholesalePrice} руб.</span>
                        <span class="discount-percent">-${product.discountPercent}%</span>
                    </div>
                    <div class="price-line">
                        <span class="price-label">Розничная:</span>
                        <span class="discount-price">${product.discountPriceRetail || product.retailPrice} руб.</span>
                    </div>
                    <div class="old-price-container">
                        <span class="original-price">${product.retailPrice} руб.</span>
                        <span class="discount-percent">-${product.discountPercent}%</span>
                    </div>
                `;
            } else {
                html += `
                    <div class="price-line">
                        <span class="price-label">Оптовая:</span>
                        <span class="price-value">${product.wholesalePrice} руб.</span>
                    </div>
                    <div class="price-line">
                        <span class="price-label">Розничная:</span>
                        <span class="price-value">${product.retailPrice} руб.</span>
                    </div>
                `;
            }
            
            if (SHOW_WHOLESALE_PLUS) {
                html += `
                    <div class="price-line">
                        <span class="price-label">Оптовая+:</span>
                        <span class="price-value">${product.wholesalePlusPrice} руб.</span>
                    </div>
                `;
            }
            
            html += '</div>';
            return html;
        }

        function formatPriceWithDiscountModal(product) {
            return formatPriceWithDiscount(product);
        }

        function formatStockInfoModal(product) {
            const stocks = product.stocks;
            const storageLocation = product.storageLocation;
            
            let html = '<div class="scan-result-stock">';
            html += '<div style="font-weight: bold; margin-bottom: 5px;">Остатки:</div>';
            
            html += `<div style="display: flex; justify-content: space-between; padding: 2px 0;">
                <span>ТОРГОВЫЙ ЗАЛ:</span>
                <span style="color: ${stocks.warehouse1 < 0 ? '#f44336' : '#2e7d32'}; font-weight: bold;">
                    ${formatNumber(stocks.warehouse1)} шт.
                </span>
            </div>`;
            
            html += `<div style="display: flex; justify-content: space-between; padding: 2px 0;">
                <span>СКЛАД:</span>
                <span style="color: ${stocks.warehouse2 < 0 ? '#f44336' : '#2e7d32'}; font-weight: bold;">
                    ${formatNumber(stocks.warehouse2)} шт.
                </span>
            </div>`;
            
            html += '</div>';
            
            if (storageLocation && storageLocation.trim() !== '') {
                html += `<div class="scan-result-storage">
                    <div style="font-weight: bold; color: #856404;">Место хранения:</div>
                    <div style="font-weight: bold;">${storageLocation}</div>
                </div>`;
            }
            
            return html;
        }

        function showScanResults(code, results) {
            lastScannedCode = code;
            
            const resultCount = document.getElementById('resultCount');
            const resultProducts = document.getElementById('resultProducts');
            
            if (results.length === 0) {
                resultCount.textContent = 'Товары не найдены';
                resultCount.style.color = '#f44336';
                resultProducts.innerHTML = '<div class="scan-result-card" style="text-align: center; color: #666;">По этому штрихкоду товары не найдены</div>';
            } else {
                const groupedResults = groupProductsByKey(results);
                resultCount.textContent = `Найдено товаров: ${results.length} (${groupedResults.length} уникальных)`;
                resultCount.style.color = '#4CAF50';
                
                resultProducts.innerHTML = '';
                
                groupedResults.forEach(product => {
                    const card = document.createElement('div');
                    card.className = 'scan-result-card';
                    
                    const hasImage = product.imageCode && product.imageCode.trim() !== '';
                    let printBtnHTML;
                    
                    if (isIOS()) {
                        printBtnHTML = `<button class="print-button" onclick="openPrintModalIOS(${JSON.stringify(product).replace(/"/g, '&quot;')})" title="Печать ценника (iOS)">🖨️</button>`;
                    } else {
                        printBtnHTML = `<button class="print-button" onclick="openPrintModal(${JSON.stringify(product).replace(/"/g, '&quot;')})" title="Печать ценника">🖨️</button>`;
                    }

                    card.innerHTML = `
                        <div style="font-size: 12px; color: #666; margin-bottom: 5px;">
                            <strong>Штрихкод:</strong> ${product.count > 1 ? `Несколько (${product.count})` : product.barcode}
                        </div>
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px;">
                            <div>
                                <strong>Артикул:</strong> ${product.article}
                                ${hasImage ? '<button class="image-button" style="margin-left: 10px;" onclick="showProductImage(' + JSON.stringify(product).replace(/"/g, '&quot;') + ')">🖼️</button>' : ''}
                            </div>
                            ${printBtnHTML}
                        </div>
                        <div style="font-size: 16px; margin-bottom: 8px;">${product.name}</div>
                        ${formatPriceWithDiscountModal(product)}
                        ${formatStockInfoModal(product)}
                    `;
                    
                    resultProducts.appendChild(card);
                });
            }
            
            document.getElementById('cameraModal').style.display = 'none';
            document.getElementById('iosScannerModal').style.display = 'none';
            document.getElementById('resultModal').style.display = 'flex';
        }

        function displayResults(results, query, searchMode) {
            const container = document.getElementById('resultsContainer');
            container.innerHTML = '';

            if (results.length === 0) {
                container.innerHTML = '<div class="no-results">Товары не найдены</div>';
                container.style.display = 'block';
                return;
            }

            const groupedResults = groupProductsByKey(results);
            
            const countEl = document.createElement('div');
            countEl.className = 'results-count';
            countEl.textContent = `Найдено товаров: ${results.length} (${groupedResults.length} уникальных)`;
            container.appendChild(countEl);

            const modeEl = document.createElement('div');
            modeEl.className = 'search-mode';
            modeEl.textContent = `Режим поиска: ${searchMode}`;
            container.appendChild(modeEl);

            groupedResults.forEach(product => {
                container.appendChild(createProductCard(product, query, searchMode));
            });

            container.style.display = 'block';
        }

        function searchProducts() {
            const mode = getCurrentSearchMode();
            
            if (mode === 'combined') {
                const articlePart = document.getElementById('articleInput').value.trim();
                const namePart = document.getElementById('nameInput').value.trim();
                const barcodePart = document.getElementById('barcodeInput').value.trim();
                
                if (!articlePart && !namePart && !barcodePart) {
                    document.getElementById('resultsContainer').style.display = 'none';
                    return;
                }
                
                const results = performCombinedSearch(articlePart, namePart, barcodePart);
                displayResults(results, { article: articlePart, name: namePart, barcode: barcodePart }, 'комбинированный');
            } else {
                const query = document.getElementById('searchInput').value.trim();
                if (!query) {
                    document.getElementById('resultsContainer').style.display = 'none';
                    return;
                }
                
                const results = performSimpleSearch(query, mode);
                displayResults(results, query, getSearchModeDisplayName(mode));
            }
        }

        function updateClearButton() {
            const mode = getCurrentSearchMode();
            let hasText = false;
            
            if (mode === 'combined') {
                hasText = document.getElementById('articleInput').value.trim() !== '' || 
                          document.getElementById('nameInput').value.trim() !== '' || 
                          document.getElementById('barcodeInput').value.trim() !== '';
            } else {
                hasText = document.getElementById('searchInput').value.trim() !== '';
            }
            
            document.getElementById('clearSearchBtn').style.display = hasText ? 'block' : 'none';
        }

        function clearSearchFields() {
            const mode = getCurrentSearchMode();
            
            if (mode === 'combined') {
                document.getElementById('articleInput').value = '';
                document.getElementById('nameInput').value = '';
                document.getElementById('barcodeInput').value = '';
            } else {
                document.getElementById('searchInput').value = '';
                document.getElementById('searchInput').focus();
            }
            
            updateClearButton();
            document.getElementById('resultsContainer').style.display = 'none';
        }

        function showProductImage(product) {
            const modal = document.createElement('div');
            modal.className = 'modal-overlay';
            modal.style.display = 'flex';
            
            let imageUrl = '';
            if (product.imageCode) {
                let fileName = product.imageCode.trim();
                if (!fileName.includes('.')) fileName += '.jpg';
                imageUrl = `https://pk.kubanstar.ru/images/virtuemart/product/${fileName}`;
            }
            
            modal.innerHTML = `
                <div class="modal-frame" style="max-width: 90%; max-height: 90%;">
                    <div style="text-align: center;">
                        <h3 style="margin-bottom: 20px;">${product.article} - ${product.name}</h3>
                        <div style="max-height: 70vh; overflow: auto;">
                            <img src="${imageUrl || 'data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg"><text x="50%" y="50%" font-size="50">🖼️</text></svg>'}" 
                                 style="max-width: 100%; max-height: 60vh; border-radius: 8px;"
                                 onerror="this.src='data:image/svg+xml,<svg xmlns=\'http://www.w3.org/2000/svg\'><text x=\'50%\' y=\'50%\' font-size=\'50\'>❌</text></svg>'">
                        </div>
                        <button onclick="this.closest('.modal-overlay').remove()" 
                                class="camera-btn" 
                                style="background-color: #f44336; margin-top: 20px;">
                            Закрыть
                        </button>
                    </div>
                </div>
            `;
            
            document.body.appendChild(modal);
        }

        function getFileDatesData() {
            let displayDate = "Дата не указана";
            if (DATA_UPDATE_DATE) {
                displayDate = DATA_UPDATE_DATE.split(" ")[0] || DATA_UPDATE_DATE;
            }
            
            return {
                currentDate: displayDate,
                files: [{
                    location: "Обмен",
                    items: [
                        { label: "СКЛАД", lastModified: URAL_OFFICE_DATE || "Дата не указана" },
                        { label: "Торговый зал", lastModified: URAL_DATE || "Дата не указана" }
                    ]
                }]
            };
        }

        function openDatesModal() {
            const data = getFileDatesData();
            const modal = document.getElementById('datesModal');
            const content = document.getElementById('datesContent');
            
            document.getElementById('modalCurrentDate').textContent = `Дата обновления: ${data.currentDate}`;
            
            let updateTime = "00:00";
            if (DATA_UPDATE_DATE && DATA_UPDATE_DATE.includes(" ")) {
                const timeMatch = DATA_UPDATE_DATE.match(/\s(\d{2}:\d{2})$/);
                if (timeMatch) updateTime = timeMatch[1];
            }
            document.getElementById('dataUpdateContainer').textContent = `Данные на : ${updateTime}`;
            
            let html = '';
            data.files.forEach(section => {
                html += `<div class="date-section"><div class="date-section-title">${section.location}:</div>`;
                section.items.forEach(item => {
                    html += `<div class="date-item">
                        <div class="date-item-row">
                            <div class="date-item-label">${item.label}:</div>
                            <div class="date-item-time">${item.lastModified}</div>
                        </div>
                    </div>`;
                });
                html += '</div>';
            });
            
            content.innerHTML = html;
            modal.style.display = 'flex';
        }

        function closeDatesModal() {
            document.getElementById('datesModal').style.display = 'none';
        }

        function updateSearchUI() {
            const mode = getCurrentSearchMode();
            const combinedFields = document.getElementById('combinedSearchFields');
            const searchInput = document.getElementById('searchInput');
            
            if (mode === 'combined') {
                combinedFields.style.display = 'flex';
                searchInput.style.display = 'none';
            } else {
                combinedFields.style.display = 'none';
                searchInput.style.display = 'block';
            }
            
            updateClearButton();
        }

        function initScrollToTopButton() {
            const btn = document.getElementById('scrollToTopBtn');
            
            window.addEventListener('scroll', () => {
                btn.classList.toggle('show', window.pageYOffset > 300);
            });
            
            btn.addEventListener('click', () => {
                window.scrollTo({ top: 0, behavior: 'smooth' });
            });
        }

        // ===== ИНИЦИАЛИЗАЦИЯ =====
        document.addEventListener('DOMContentLoaded', function() {
            // Инициализация UI
            updateSearchUI();
            setupPlatformUI();
            initScrollToTopButton();
            
            // Установка даты
            if (DATA_UPDATE_DATE) {
                document.getElementById('current-date').textContent = DATA_UPDATE_DATE.split(" ")[0] || DATA_UPDATE_DATE;
            }
            
            // Обработчики поиска
            document.getElementById('searchButton').addEventListener('click', searchProducts);
            document.getElementById('clearSearchBtn').addEventListener('click', clearSearchFields);
            
            // Обработчики ввода
            document.getElementById('searchInput').addEventListener('input', updateClearButton);
            document.getElementById('articleInput').addEventListener('input', updateClearButton);
            document.getElementById('nameInput').addEventListener('input', updateClearButton);
            document.getElementById('barcodeInput').addEventListener('input', updateClearButton);
            
            // Enter для поиска
            ['searchInput', 'articleInput', 'nameInput', 'barcodeInput'].forEach(id => {
                document.getElementById(id).addEventListener('keydown', (e) => {
                    if (e.key === 'Enter') searchProducts();
                });
            });
            
            // Обработчики сканирования
            document.getElementById('scanButtonAndroid').addEventListener('click', openCamera);
            document.getElementById('scanButtonIOS').addEventListener('click', openIOSScanner);
            
            // Обработчики камеры
            document.getElementById('closeCameraModal').addEventListener('click', () => {
                stopCameraStream();
                document.getElementById('cameraModal').style.display = 'none';
            });
            
            document.getElementById('stopCamera').addEventListener('click', () => {
                stopCameraStream();
                document.getElementById('cameraModal').style.display = 'none';
            });
            
            // Обработчики результатов
            document.getElementById('continueScanBtn').addEventListener('click', () => {
                document.getElementById('resultModal').style.display = 'none';
                setTimeout(() => {
                    if (isIOS()) openIOSScanner();
                    else openCamera();
                }, 300);
            });
            
            document.getElementById('closeResultBtn').addEventListener('click', () => {
                document.getElementById('resultModal').style.display = 'none';
            });
            
            // Обработчики печати Android
            document.getElementById('closePrintModal').addEventListener('click', closePrintModal);
            document.getElementById('printActionBtn').addEventListener('click', handlePrint);
            
            // Обработчики печати iOS
            if (isIOS()) {
                document.getElementById('scanPrintersBtnIOS').addEventListener('click', scanForBLEPrinters);
                document.getElementById('printActionBtnIOS').addEventListener('click', handlePrintIOS);
                document.getElementById('closePrintModalIOS').addEventListener('click', closePrintModalIOS);
                
                // Восстановление сохраненного принтера
                try {
                    savedBluetoothDeviceId = localStorage.getItem('savedPrinterId');
                    if (savedBluetoothDeviceId) {
                        console.log('Найден сохраненный принтер');
                    }
                } catch (e) {
                    console.warn('Не удалось прочитать localStorage');
                }
            }
            
            // Обработчики iOS сканера
            document.getElementById('closeIOSScanner').addEventListener('click', closeIOSScanner);
            document.getElementById('iosScannerModal').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) closeIOSScanner();
            });
            
            // Обработчики дат
            document.getElementById('current-date').addEventListener('click', openDatesModal);
            document.getElementById('closeDatesModal').addEventListener('click', closeDatesModal);
            
            // Обработчик переключения режимов поиска
            document.querySelectorAll('input[name="searchMode"]').forEach(radio => {
                radio.addEventListener('change', function() {
                    updateSearchUI();
                    if (document.getElementById('searchInput').value.trim() || 
                        (getCurrentSearchMode() === 'combined' && 
                         (document.getElementById('articleInput').value.trim() || 
                          document.getElementById('nameInput').value.trim() || 
                          document.getElementById('barcodeInput').value.trim()))) {
                        searchProducts();
                    }
                });
            });
            
            // Обработчик Escape
            document.getElementById('searchInput').addEventListener('keydown', (e) => {
                if (e.key === 'Escape') clearSearchFields();
            });
            
            // Обработчик Ctrl+F
            document.addEventListener('keydown', (e) => {
                if ((e.ctrlKey || e.metaKey) && e.key === 'f') {
                    e.preventDefault();
                    if (getCurrentSearchMode() === 'combined') {
                        document.getElementById('articleInput').focus();
                    } else {
                        document.getElementById('searchInput').focus();
                        document.getElementById('searchInput').select();
                    }
                }
            });
            
            // Обработчики видимости для iOS
            if (isIOS()) {
                document.addEventListener('visibilitychange', () => {
                    if (document.hidden && iosIsScanning) closeIOSScanner();
                });
                
                window.addEventListener('orientationchange', () => {
                    if (iosIsScanning) {
                        closeIOSScanner();
                        setTimeout(openIOSScanner, 500);
                    }
                });
            }
            
            // Обработчики модальных окон
            document.getElementById('cameraModal').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) {
                    stopCameraStream();
                    e.currentTarget.style.display = 'none';
                }
            });
            
            document.getElementById('resultModal').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) e.currentTarget.style.display = 'none';
            });
            
            document.getElementById('printModal').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) closePrintModal();
            });
            
            document.getElementById('printModalIOS').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) closePrintModalIOS();
            });
            
            document.getElementById('datesModal').addEventListener('click', (e) => {
                if (e.target === e.currentTarget) closeDatesModal();
            });
        });
    </script>
</body>
</html>