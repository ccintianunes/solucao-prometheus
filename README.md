## Solução-prometheus
Criando um ambiente de monitonamento com Prometheus e Grafana

## Criando o serviço Prometheus pelo Docker

### 1. Crie um diretório para armazenar os arquivos de configuração e dados do Prometheus.

(diretório sugerido: /opt/containerd )
```
mkdir /opt/containerd/prometheus
cd /opt/containerd/prometheus
```
### 2. Crie um arquivo de configuração do Prometheus chamado `prometheus.yml`,

 a) Para incluir uma máquina virtual como um alvo de coleta do Prometheus é preciso declara o IP dela no arquivo `prometheus.yml` da seguinte forma:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['192.168.10.1:9100']
```

 b) Este arquivo de configuração configura o Prometheus para coletar métricas de si mesmo e da **máquina com IP 192.168.10.1**, a qual queremos, por exemplo, observar.:
  > _'targets: ['localhost:9090']'_ Mapeia a porta 9090 do host para a porta 9090 do container



### 3. Inicie o Prometheus usando o Docker com o seguinte comando:

_Certifique-se de substituir o caminho "opt/containerd/prometheus/" 
   pelo caminho real para o diretório que você criou no Passo 1._
```
docker run -d -p 9090:9090 \
 --name prometheus \
 -v /opt/containerd/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml \
 prom/prometheus
```

  

4. O Prometheus está acessível na URL: http://IP-do-host:9090
