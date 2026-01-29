# Escalabilidad y Nuevas Tecnologías en Bitcoin (Universidad de las Hesperides)

## Clase sincrónica 1 (Experimentando con LN)

1. Empezar a descargarse Polar (https://lightningpolar.com/)
2. Como es mejor para comunicarse entre el grupo
3. Explicar método socrático para proxima clase

### Revisar diagramas apertura canales de LN

### Ver la red en el mundo real (amboss.space)

#### Información del Par

- Peer: El alias/nombre del nodo remoto con el que se abrió el canal (por ejemplo, bfx-lnd0 es el nodo Lightning de Bitfinex, fixedfloat.com es un servicio de intercambio).
- Channels: Número total de canales que ese nodo par tiene en toda la red, no solo con ACINQ.
- Capacity: Capacidad total de ese nodo par sumando todos sus canales en la red, expresada en satoshis y BTC.

#### Información del Canal

- Id: El identificador corto del canal, codificado como altura_de_bloque x índice_tx x índice_salida (ej: 851844x1689x0). Identifica de forma única la transacción de financiamiento del canal en la blockchain. El número grande al lado es el mismo ID en formato entero de 64 bits.
- Capacity: La cantidad total de satoshis bloqueados en este canal específico (ej: 500,000,000 sats = 5 BTC). Es el máximo que puede enrutarse a través de él.
- Age: Cuánto tiempo lleva abierto el canal desde que su transacción de financiamiento fue confirmada (ej: 1y 207d 3h = 1 año, 207 días, 3 horas).
- Cost: La comisión on-chain pagada (en sats) para abrir el canal al transmitir la transacción de financiamiento a la blockchain.
- Type: Si se abrió individualmente o como parte de una transacción por lotes (ej: "Batch (5)" significa que 5 canales se abrieron en una sola transacción para ahorrar comisiones).
- Transaction: El ID de la transacción on-chain de financiamiento (truncado) y el índice de salida que creó este canal.

#### Política de Comisiones - Lado de ACINQ (columnas izquierdas)

- Rate (ppm): La tasa de comisión proporcional en partes por millón. Una tasa de 1,999 ppm significa 1,999 sats por cada 1,000,000 de sats enrutados (≈0.2%).
- Inbound Rate (ppm): Tasa de comisión aplicada a la liquidez entrante (pagos que fluyen hacia ACINQ). Un - o 0 significa que no hay cargo adicional por dirección entrante.
- Base (sat): Una comisión base fija cobrada por cada pago reenviado, independientemente del monto. Aquí es 1 sat.
- Inbound Base (sat): Comisión base fija para pagos entrantes. 0 significa sin comisión base adicional entrante.
- Time Lock Delta: El número de bloques que ACINQ requiere como margen de seguridad en la cadena de timelocks de los HTLC. 144 bloques ≈ 1 día. Es el delta CLTV (CheckLockTimeVerify) — le da tiempo al nodo para reclamar fondos on-chain si algo sale mal.
- Max HTLC (sats): El tamaño máximo de pago (un solo HTLC) que este canal reenviará. Valores como 10,000 o 400,000 sats.
- Min HTLC (sats): El tamaño mínimo de pago aceptado. 0.001 sat (1 millisatoshi) es el mínimo del protocolo.

#### Política de Comisiones — Lado del Par (columnas derechas)

- Rate (ppm): La comisión proporcional del par (ej: 1 ppm o 250 ppm).
- Base (sat): La comisión base del par (ej: 1 sat o 0.001 sat).
- Time Lock Delta: El delta CLTV requerido por el par (ej: 80 o 51 bloques).
- Max HTLC (sats): Monto máximo reenviable por el par (20,000,000 sats = 0.2 BTC).
- Min HTLC (sats): Mínimo del par (1 msat).

#### Puntos Clave

Cada canal Lightning tiene dos políticas de comisiones independientes — una por dirección — porque cada nodo establece sus propias comisiones de enrutamiento. La combinación de comisión base + tasa proporcional (ppm) determina el costo total de enrutar un pago: comisión_total = base + (monto × tasa / 1,000,000). El timelock delta y los valores máximo/mínimo de HTLC son restricciones de seguridad y capacidad que determinan qué pagos pueden usar ese canal para enrutamiento.

### Experimentar con polar (https://lightningpolar.com/)

1. Crear una red
2. Comprar contenedores docker
3. Agregar par
4. Verificación del saldo de la billetera
5. Obtener fondos en la billetera: "lncli newaddress p2wkh"
6. Apertura manual de canal
7. Apertura de canal programada (index.js)
8. Pago de factura Bolt11
