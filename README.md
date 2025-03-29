# Pacman Tracking and Inference

Este proyecto implementa un sistema de inferencia probabilística para rastrear la posición de fantasmas en el juego de Pacman. A continuación, se describen las principales modificaciones y funcionalidades implementadas en el archivo `inference.py`.

## Clases y Métodos Implementados

### 1. `DiscreteDistribution`
Esta clase modela distribuciones discretas de creencias y pesos. Es una subclase de `dict` que incluye métodos adicionales para trabajar con distribuciones probabilísticas.

- **`normalize()`**: Normaliza la distribución para que la suma de los valores sea igual a 1. Si la suma es 0, no realiza ninguna acción.
- **`sample()`**: Toma una muestra aleatoria de la distribución, ponderada por los valores asociados con cada clave.

### 2. `InferenceModule`
Esta clase base rastrea una distribución de creencias sobre la ubicación de un fantasma.

- **`getObservationProb(noisyDistance, pacmanPosition, ghostPosition, jailPosition)`**: Calcula la probabilidad \( P(\text{noisyDistance} | \text{pacmanPosition}, \text{ghostPosition}) \) utilizando la distancia de Manhattan y la probabilidad de observación proporcionada por `busters.getObservationProbability`.

### 3. `ParticleFilter`
Un filtro de partículas para rastrear aproximadamente la ubicación de un solo fantasma.

- **`initializeUniformly(gameState)`**: Inicializa una lista de partículas distribuidas uniformemente en las posiciones legales del tablero.
- **`getBeliefDistribution()`**: Convierte la lista de partículas en una distribución de creencias normalizada.
- **`observeUpdate(observation, gameState)`**: Actualiza las creencias basándose en la observación de distancia ruidosa y la posición de Pacman. Si todas las partículas tienen peso cero, reinicia las partículas uniformemente.
- **`elapseTime(gameState)`**: Predice el siguiente estado de cada partícula basándose en el modelo de transición.

### 4. Cambios en los Nombres de Variables
Se tradujeron los nombres de las variables al español para mayor claridad. Por ejemplo:
- `particles` → `particulas`
- `weights` → `pesos`
- `gameState` → `estadoJuego`
- `noisyDistance` → `distanciaRuidosa`

## Ejemplo de Flujo de Trabajo

1. **Inicialización**: 
   - El filtro de partículas se inicializa uniformemente en todas las posiciones legales del tablero.
   - Cada partícula representa una posible ubicación del fantasma.

2. **Actualización de Observaciones**:
   - Se calcula la probabilidad de cada partícula basándose en la distancia ruidosa observada y la posición de Pacman.
   - Las partículas se re-muestrean según sus pesos.

3. **Paso de Tiempo**:
   - Cada partícula se mueve a su siguiente estado basándose en el modelo de transición.

4. **Distribución de Creencias**:
   - Se genera una distribución de creencias normalizada que representa la probabilidad de que el fantasma esté en cada posición.

## Notas Adicionales

- **`busters.getObservationProbability`**: Calcula la probabilidad de observar una distancia ruidosa dada la distancia real.
- **`manhattanDistance`**: Se utiliza para calcular la distancia entre dos posiciones en el tablero.
- **`getPositionDistribution`**: Proporciona una distribución de probabilidad sobre las posibles posiciones futuras de un fantasma.

## Cómo Ejecutar
Para probar las funcionalidades, puedes ejecutar el juego de Pacman con el siguiente comando:
```bash
python pacman.py -p BustersAgent -l smallGrid -g RandomGhost -z 0.5
```
- `-p BustersAgent`: Utiliza un agente que rastrea fantasmas.
- `-l smallGrid`: Usa un tablero pequeño.
- `-g RandomGhost`: Los fantasmas se mueven aleatoriamente.
- `-z 0.5`: Ajusta el ruido de las observaciones.

## Contribuciones
Este proyecto se basa en los proyectos de IA de UC Berkeley. Las modificaciones realizadas incluyen la implementación de métodos de inferencia probabilística y la traducción de variables al español para facilitar la comprensión.