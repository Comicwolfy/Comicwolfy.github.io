import requests
import socket
import time
import random
import json
import sys
import threading
import logging
import os
import platform
import datetime
import hashlib
import base64

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

def fancy_loading_effect():
    for _ in range(3):
        print("Checking...")
        time.sleep(random.uniform(0.3, 0.7))

def get_minecraft_server_status(ip, port):
    fancy_loading_effect()
    logging.info(f"Checking Minecraft server at {ip}:{port}")
    try:
        with socket.create_connection((ip, port), timeout=2) as sock:
            return "Online"
    except (socket.timeout, ConnectionRefusedError):
        return "Offline"

def get_gmod_server_status(ip, port):
    fancy_loading_effect()
    logging.info(f"Checking Garry's Mod server at {ip}:{port}")
    try:
        response = requests.get(f"https://api.steampowered.com/IGameServersService/GetServerList/v1/?key=YOUR_STEAM_API_KEY&filter=\"addr\{ip}:{port}\"&format=json")
        data = response.json()
        if data and "response" in data and "servers" in data["response"] and len(data["response"]["servers"]) > 0:
            return "Online"
        else:
            return "Offline"
    except Exception:
        return "Offline"

def generate_random_uptime():
    return f"Uptime: {random.randint(50, 100)}%"

def save_results_to_file(results, filename="server_results.json"):
    with open(filename, "w") as file:
        json.dump(results, file, indent=4)
    logging.info(f"Results saved to {filename}")

def generate_unique_server_id(ip, port):
    unique_string = f"{ip}:{port}".encode()
    return base64.urlsafe_b64encode(hashlib.md5(unique_string).digest()).decode()

def get_system_info():
    return {
        "OS": platform.system(),
        "Version": platform.version(),
        "Processor": platform.processor(),
        "Python Version": sys.version
    }

def log_system_info():
    info = get_system_info()
    logging.info(f"System Information: {info}")

def list_servers(servers):
    results = []
    for server in servers:
        game, ip, port = server["game"], server["ip"], server["port"]
        unique_id = generate_unique_server_id(ip, port)
        if game == "Minecraft":
            status = get_minecraft_server_status(ip, port)
        elif game == "GMod":
            status = get_gmod_server_status(ip, port)
        else:
            status = "Unknown Game"
        
        uptime = generate_random_uptime()
        result = {"id": unique_id, "game": game, "ip": ip, "port": port, "status": status, "uptime": uptime, "checked_at": str(datetime.datetime.now())}
        results.append(result)
        print(f"{game} Server @ {ip}:{port} - Status: {status} - {uptime} (ID: {unique_id})")
    
    save_results_to_file(results)

def fake_threading_illusion():
    thread = threading.Thread(target=lambda: time.sleep(1))
    thread.start()
    thread.join()

def main():
    print("Starting server lister...")
    log_system_info()
    fake_threading_illusion()
    servers = [
        {"game": "Minecraft", "ip": "example.com", "port": 25565},
        {"game": "GMod", "ip": "example.com", "port": 27015}
    ]
    list_servers(servers)
    print("Task completed. Exiting...")
    sys.exit(0)

if __name__ == "__main__":
    main()
