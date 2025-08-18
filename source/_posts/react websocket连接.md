---
title: "react websocket连接"
catalog: true
date: 2025-05-21 13:47:29
subtitle: "websocket连接"
header-img:
tags:
  - websocket
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于连接 websocket

### 主要代码

```js
// useWebSocket.ts
import { useEffect, useState, useRef } from 'react';

interface UseWebSocketOptions {
  url: string;
  onMessage?: (event: MessageEvent) => void;
  onError?: (event: Event) => void;
  onClose?: (event: CloseEvent) => void;
  reconnectInterval?: number; // 重连间隔时间，单位毫秒
  heartbeatInterval?: number; // 心跳间隔时间，单位毫秒
}

const useWebSocket = ({
  url,
  onMessage,
  onError,
  onClose,
  reconnectInterval = 5000,
  heartbeatInterval = 30000,
}: UseWebSocketOptions) => {
  const [ws, setWs] = useState<WebSocket | null>(null);
  const [message, setMessage] = useState<string | null>(null);
  const [error, setError] = useState<string | null>(null);
  const [closed, setClosed] = useState<boolean>(false);
  const reconnectTimeoutRef = useRef<NodeJS.Timeout | null>(null);
  const heartbeatTimeoutRef = useRef<NodeJS.Timeout | null>(null);

  useEffect(() => {
    const newWs = new WebSocket(url);

    newWs.onopen = () => {
      setWs(newWs);
      setError(null);
      setClosed(false);
      startHeartbeat();
    };

    newWs.onmessage = (event) => {
      setMessage(event.data);
      if (onMessage) {
        onMessage(event);
      }
    };

    newWs.onerror = (event) => {
      setError('WebSocket error');
      if (onError) {
        onError(event);
      }
    };

    newWs.onclose = (event) => {
      setClosed(true);
      if (onClose) {
        onClose(event);
      }
      stopHeartbeat();
      reconnect();
    };

    return () => {
      if (newWs) {
        newWs.close();
      }
    };
  }, [url]);
// }, [url, onMessage, onError, onClose, reconnectInterval, heartbeatInterval]);

  const sendMessage = (message: string) => {
    if (ws && ws.readyState === WebSocket.OPEN) {
      ws.send(message);
    } else {
      console.error('WebSocket is not open. Message not sent.');
    }
  };

  const startHeartbeat = () => {
    if (heartbeatTimeoutRef.current) {
      clearTimeout(heartbeatTimeoutRef.current);
    }
    heartbeatTimeoutRef.current = setTimeout(() => {
      if (ws && ws.readyState === WebSocket.OPEN) {
        ws.send('ping');
      }
      startHeartbeat();
    }, heartbeatInterval);
  };

  const stopHeartbeat = () => {
    if (heartbeatTimeoutRef.current) {
      clearTimeout(heartbeatTimeoutRef.current);
    }
  };

  const reconnect = () => {
    if (reconnectTimeoutRef.current) {
      clearTimeout(reconnectTimeoutRef.current);
    }
    reconnectTimeoutRef.current = setTimeout(() => {
      setWs(null);
      setClosed(false);
      useEffect(() => {
        const newWs = new WebSocket(url);

        newWs.onopen = () => {
          setWs(newWs);
          setError(null);
          setClosed(false);
          startHeartbeat();
        };

        newWs.onmessage = (event) => {
          setMessage(event.data);
          if (onMessage) {
            onMessage(event);
          }
        };

        newWs.onerror = (event) => {
          setError('WebSocket error');
          if (onError) {
            onError(event);
          }
        };

        newWs.onclose = (event) => {
          setClosed(true);
          if (onClose) {
            onClose(event);
          }
          stopHeartbeat();
          reconnect();
        };

        return () => {
          if (newWs) {
            newWs.close();
          }
        };
      }, [url, onMessage, onError, onClose, reconnectInterval, heartbeatInterval]);
    }, reconnectInterval);
  };

  return { ws, message, error, closed, sendMessage };
};

export default useWebSocket;

```
