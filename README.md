<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatApp - Platform Chat Online</title>
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>💬</text></svg>">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        :root {
            --primary: #4361ee;
            --secondary: #3a0ca3;
            --accent: #7209b7;
            --light: #f8f9fa;
            --dark: #212529;
            --success: #4cc9f0;
            --danger: #f72585;
            --warning: #f8961e;
            --info: #4895ef;
            --online: #4ade80;
            --offline: #94a3b8;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: var(--dark);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 15px;
            height: 100%;
        }

        /* Header Styles */
        header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            color: var(--dark);
            padding: 1rem 0;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 1.8rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            color: var(--primary);
        }

        .logo i {
            margin-right: 10px;
        }

        nav ul {
            display: flex;
            list-style: none;
        }

        nav ul li {
            margin-left: 20px;
        }

        nav ul li a {
            color: var(--dark);
            text-decoration: none;
            font-weight: 500;
            padding: 8px 16px;
            border-radius: 20px;
            transition: all 0.3s;
        }

        nav ul li a:hover, nav ul li a.active {
            background-color: var(--primary);
            color: white;
        }

        .auth-buttons {
            display: flex;
            gap: 10px;
        }

        .btn {
            padding: 8px 20px;
            border: none;
            border-radius: 20px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
        }

        .btn-outline {
            background-color: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
        }

        .btn-success {
            background-color: var(--success);
            color: white;
        }

        .btn-danger {
            background-color: var(--danger);
            color: white;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        /* Main Content */
        .main-content {
            flex: 1;
            display: flex;
            padding: 20px 0;
            min-height: calc(100vh - 140px);
        }

        .welcome-section {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            color: white;
            padding: 40px 20px;
        }

        .welcome-section h1 {
            font-size: 3rem;
            margin-bottom: 1rem;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .welcome-section p {
            font-size: 1.2rem;
            margin-bottom: 2rem;
            max-width: 600px;
            opacity: 0.9;
        }

        .welcome-features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 2rem;
            width: 100%;
            max-width: 800px;
        }

        .feature-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 20px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .feature-card h3 {
            margin-bottom: 10px;
            font-size: 1.3rem;
        }

        /* Chat Interface */
        .chat-interface {
            display: none;
            flex: 1;
            gap: 20px;
        }

        .sidebar {
            width: 300px;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            padding: 20px;
            overflow-y: auto;
            height: calc(100vh - 180px);
            position: sticky;
            top: 100px;
        }

        .chat-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            height: calc(100vh - 180px);
        }

        .chat-header {
            padding: 15px 20px;
            background-color: var(--light);
            border-bottom: 1px solid #e9ecef;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
            background: #f8f9fa;
        }

        .message {
            max-width: 70%;
            padding: 12px 16px;
            border-radius: 18px;
            position: relative;
            word-wrap: break-word;
            animation: messageAppear 0.3s ease-out;
        }

        @keyframes messageAppear {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .message.sent {
            align-self: flex-end;
            background-color: var(--primary);
            color: white;
            border-bottom-right-radius: 5px;
        }

        .message.received {
            align-self: flex-start;
            background-color: white;
            color: var(--dark);
            border-bottom-left-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .message-sender {
            font-size: 0.8rem;
            margin-bottom: 5px;
            opacity: 0.8;
            font-weight: 600;
        }

        .message-text {
            margin-bottom: 5px;
            line-height: 1.4;
        }

        .message-time {
            font-size: 0.7rem;
            text-align: right;
            margin-top: 5px;
            opacity: 0.7;
        }

        .message-media {
            margin-top: 8px;
            max-width: 100%;
            border-radius: 8px;
            overflow: hidden;
        }

        .message-media img {
            max-width: 100%;
            max-height: 300px;
            border-radius: 8px;
            display: block;
        }

        .message-media video {
            max-width: 100%;
            max-height: 300px;
            border-radius: 8px;
            display: block;
        }

        .message-audio {
            margin-top: 8px;
            width: 100%;
        }

        .message-audio audio {
            width: 100%;
            max-width: 300px;
        }

        .chat-input {
            padding: 15px 20px;
            background-color: white;
            border-top: 1px solid #e9ecef;
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .chat-input input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #ced4da;
            border-radius: 20px;
            outline: none;
            transition: border-color 0.3s;
            font-size: 1rem;
        }

        .chat-input input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(67, 97, 238, 0.2);
        }

        .media-buttons {
            display: flex;
            gap: 5px;
        }

        .media-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: var(--primary);
            padding: 8px;
            border-radius: 50%;
            transition: background-color 0.3s;
        }

        .media-btn:hover {
            background-color: rgba(67, 97, 238, 0.1);
        }

        .chat-input button {
            background-color: var(--primary);
            color: white;
            border: none;
            border-radius: 50%;
            width: 45px;
            height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .chat-input button:hover {
            background-color: var(--secondary);
            transform: scale(1.05);
        }

        /* Tabs */
        .tab-container {
            display: flex;
            border-bottom: 1px solid #e9ecef;
            margin-bottom: 15px;
        }

        .tab {
            padding: 10px 20px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s;
            border-bottom: 2px solid transparent;
        }

        .tab.active {
            color: var(--primary);
            border-bottom: 2px solid var(--primary);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        /* Contact and Group Lists */
        .contact-list, .group-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .contact, .group {
            padding: 12px 15px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            border: 1px solid transparent;
        }

        .contact:hover, .group:hover {
            background-color: #f8f9fa;
            border-color: #e9ecef;
            transform: translateX(5px);
        }

        .contact.active, .group.active {
            background-color: #e7f1ff;
            border-color: var(--primary);
        }

        .contact-avatar, .group-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            margin-right: 10px;
            flex-shrink: 0;
        }

        .contact-info, .group-info {
            flex: 1;
            min-width: 0;
        }

        .contact-name, .group-name {
            font-weight: 500;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .contact-status, .group-members {
            font-size: 0.8rem;
            color: #6c757d;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .online-indicator {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: var(--online);
            margin-left: auto;
            flex-shrink: 0;
            box-shadow: 0 0 0 2px white;
        }

        .offline-indicator {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: var(--offline);
            margin-left: auto;
            flex-shrink: 0;
            box-shadow: 0 0 0 2px white;
        }

        /* Action Buttons */
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        .action-btn {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 15px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
            cursor: pointer;
            transition: all 0.3s;
            border: 1px solid #e9ecef;
        }

        .action-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border-color: var(--primary);
        }

        .action-icon {
            font-size: 1.5rem;
            margin-bottom: 10px;
            color: var(--primary);
        }

        .action-text {
            font-weight: 500;
            font-size: 0.9rem;
            text-align: center;
        }

        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background-color: white;
            border-radius: 15px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            animation: modalAppear 0.3s ease-out;
        }

        @keyframes modalAppear {
            from {
                opacity: 0;
                transform: scale(0.9) translateY(-20px);
            }
            to {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }

        .modal-header {
            padding: 15px 20px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-body {
            padding: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .form-group input, .form-group textarea {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ced4da;
            border-radius: 8px;
            outline: none;
            transition: all 0.3s;
            font-size: 1rem;
        }

        .form-group input:focus, .form-group textarea:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(67, 97, 238, 0.2);
        }

        .modal-footer {
            padding: 15px 20px;
            background-color: #f8f9fa;
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }

        .user-id {
            background-color: #e9ecef;
            padding: 8px 12px;
            border-radius: 6px;
            font-family: monospace;
            font-size: 0.9rem;
            margin-top: 5px;
            border: 1px solid #dee2e6;
        }

        /* Notification */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 10px;
            background-color: white;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
            z-index: 1100;
            display: flex;
            align-items: center;
            gap: 10px;
            transform: translateX(150%);
            transition: transform 0.3s ease;
            max-width: 400px;
        }

        .notification.show {
            transform: translateX(0);
        }

        .notification.success {
            border-left: 4px solid var(--success);
        }

        .notification.error {
            border-left: 4px solid var(--danger);
        }

        .notification.warning {
            border-left: 4px solid var(--warning);
        }

        .notification-icon {
            font-size: 1.2rem;
        }

        .notification.success .notification-icon {
            color: var(--success);
        }

        .notification.error .notification-icon {
            color: var(--danger);
        }

        .notification.warning .notification-icon {
            color: var(--warning);
        }

        /* Footer */
        footer {
            background-color: var(--dark);
            color: white;
            padding: 20px 0;
            text-align: center;
            margin-top: auto;
        }

        .file-input {
            display: none;
        }

        .close-modal {
            cursor: pointer;
            font-size: 1.5rem;
            font-weight: bold;
            transition: transform 0.3s;
        }

        .close-modal:hover {
            transform: scale(1.1);
        }

        /* Loading */
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            backdrop-filter: blur(5px);
        }

        .loading-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
        }

        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid var(--primary);
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Error States */
        .error-message {
            color: var(--danger);
            font-size: 0.8rem;
            margin-top: 5px;
            display: none;
        }

        .form-group.error input {
            border-color: var(--danger);
        }

        .form-group.error .error-message {
            display: block;
        }

        /* Confirmation Modal */
        .confirmation-modal .modal-content {
            max-width: 400px;
        }

        .confirmation-modal .modal-body {
            text-align: center;
            padding: 30px 20px;
        }

        .confirmation-modal .modal-footer {
            justify-content: center;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
                margin-right: 0;
                margin-bottom: 20px;
                height: auto;
                position: static;
            }
            
            .message {
                max-width: 85%;
            }

            .welcome-section h1 {
                font-size: 2rem;
            }

            .welcome-section p {
                font-size: 1rem;
            }

            nav ul {
                display: none;
            }

