# Evidencia-1

## Situación Problema
El problema que se trata de simular es el como se comporta una en crecimiento poblacion 
cuando esta se encuentra con una infecion que puede ocacionar decesos debido al 
alto crecimento, bajo crecimento o debido a la enfemedad.

## Agentes utilizados
En este caso solo se creo un agente el cual es GameLifeAgent el cual puede variar entre los estados de vivo, enfermo o muerto.
Los cuales se actualizan cada iteracion de la simulacion y que por medio de checar a los vecionos adyacentes de cada agente se especififican dependiendo de si se cumple alguna de las reglas que espesificamos las cuales son:

1. Cualquier celda viva con menos de dos vecinos vivos muere, debido a la subpoblación.
2. Cualquier celda viva con dos o tres vecinos vivos sobrevive para la siguiente generación.
3. Cualquier celda con más de tres vecinos vivos muere, debido a la sobrepoblación.
4. Cualquier celda muerta con exactamente tres vecinos vivos se convierte en una celda viva, debido a la reproducción.
5. Cualquier celda viva con al menos un vecino infectado se infecta, lo contagia.
6. cualquier celda infectada sola o con 3 celdas vivas se cura, aislamiento.
7. cualquier celda infectada con mas de 3 celdas infectadas se muere, nadie lo cuida.
8. cualquier celda infectada con mas de una celda infectada continua infectada.

Primero creamos el agente y lo inicialomaos con un valor aleatorio el cual nos dice el estado con el cual va empezar.
### Codigo
```py
class GameLifeAgent(Agent):
    def __init__(self, unique_id, model):
        super().__init__(unique_id, model)
        self.live = np.random.choice([0,1,2])
        self.next_state = None
```

tambien cuenta con dos acciones las cuales son step y advance. En el caso de steo esta va checando el estado de sus vecionos para verificar cual de las reglas se cumple y cambiar el estado del agente segun cual se cumpla.

### Codigo
```py
def step(self):
        neighbours = self.model.grid.get_neighbors(
            self.pos,
            moore=True,
            include_center=False)
        live_neighbours = 0
        for neighbor in neighbours:
            live_neighbours = live_neighbours + neighbor.live
        infected_neighbours = 0
        for neighbor in neighbours:
            infected_neighbours = infected_neighbours + neighbor.live
        self.next_state = self.live
        if self.next_state == 2 and (infected_neighbours > 2):
            self.next_state = 0
        else:
            if self.next_state ==2 and (infected_neighbours < 1) and (live_neighbours < 1):
                self.next_state = 1
            else:
                if self.next_state == 1 and (infected_neighbours > 1):
                    self.next_state = 2
                else:
                    if self.next_state == 1 and (live_neighbours < 2 or live_neighbours > 3):
                        self.next_state = 0
                    else:
                        if live_neighbours == 3:
                            self.next_state = 1
```
Para la accion advance simplemente se actualiza el estado del agente dependiendo del valor obtedido en step

### Codigo
```py
def advance(self):
        """
        Define el nuevo estado calculado del método step.
        """
        self.live = self.next_state
```

## Comunicacion entre agentes
En esta simulacion solo se crea un tipo de agente, pero estos interacuan al checar sus estados, 
para poder actualizarse por lo que el estado de cada agente dependde del estado de sus vecinos

## Entornos
El entorno en esta simulacion de esta conformado por los agentes los cuales se pueden inicializar en tres diferentes estados y son actualizados dependiendo de su vecinos los cuales conforman el entrono de todoa la sumiulacion y es debido a esto que el entorno siempre se esta actualizando.

## Variables  y  los  parámetros 
Las varibles utilizadoas para el funcionamiento de esta simulacion son:

+ Tamaño del gird: define eltamaño de grid y numero de agentes.
+ Numero de Generaciones: define la cantidad de veces que se van actualizar los estados de los agentes.
+ self.live: este define el estado del agente
+ self.next_state: este actualiza el estado de los agentes.
+ live_neighbours: nos ayuda a conocer la cantidad de vecionos vivos de un agente
+ infected_neighbours: nos ayuda a conocer la cantidad de vecionos infectados de un agente


## Proceso de simulacion
El procesod de simulacion funciona por medio de la catualizacion de los estados de los agentes, el cual se define por el numero de generaciones y por el tamaño con el del grid el cual define tambien el numero de agentes que se van a simular.

## Resultados
Los resultados obtienidos de la simulacion se pueden visualizar por medio de graficas, las cuales al observalros podemos ver como al principio la simulacion empieza con muchos agentes vivos los cuales en las primeras simulaciones se van infectado y despues mueren, despues de esta primeras generacions la poblacion va aumentadno hasta alcanzar una poblacion estable que varia de foma  muy pequeña.

## GITHUB
https://github.com/A01384318/Evidencia-1.git
