# Кастомная работа агента EDGE Portainer в режиме когда бекенд за Tunnels Cloudflare


## Посмотреть переменные окружения контейнера edge
docker inspect $(docker ps | grep edge | awk '{print $1}') | grep -A 20 "Env"

## Расшифровка старого ключа:
echo "BASE64" | base64 -d

## Ставим агента на удаленный сервер
docker rm -f portainer_edge_agent && \
docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /var/lib/docker/volumes:/var/lib/docker/volumes \
  -v /:/host \
  -v portainer_agent_data:/data \
  --restart always \
  -e EDGE=1 \
  -e EDGE_ID="ВАШ_EDGE_ID" \
  -e EDGE_KEY="$(echo -n 'https://portainer.gl.sy|5.00.06.002:8000|c4SAfu/Jj+-BASE64-CXug=|9' | base64 | tr -d '\r\n=')" \
  -e EDGE_INSECURE_POLL=1 \
  --name portainer_edge_agent \
  portainer/agent:2.27.4
