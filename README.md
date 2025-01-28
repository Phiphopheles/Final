import psutil
import time
from datetime import datetime

# Función para mostrar el uso de CPU, memoria y disco
def mostrar_estadisticas():
    print("\n--- Estadísticas del Sistema ---")
    # Uso de CPU
    uso_cpu = psutil.cpu_percent(interval=1)
    print(f"Uso de CPU: {uso_cpu}%")

    # Uso de memoria
    memoria = psutil.virtual_memory()
    print(f"Uso de Memoria: {memoria.percent}% (Usado: {memoria.used / (1024**3):.2f} GB / Total: {memoria.total / (1024**3):.2f} GB)")

    # Uso de disco
    disco = psutil.disk_usage('/')
    print(f"Uso de Disco: {disco.percent}% (Usado: {disco.used / (1024**3):.2f} GB / Total: {disco.total / (1024**3):.2f} GB)")

# Función principal para actualizar y registrar las métricas
def monitorizar_sistema(duracion=30, intervalo=5):
    print("Iniciando monitorización del sistema...")
    inicio = datetime.now()
    
    try:
        while (datetime.now() - inicio).seconds < duracion:
            mostrar_estadisticas()
            time.sleep(intervalo)
    except KeyboardInterrupt:
        print("\nMonitorización detenida por el usuario.")
    finally:
        print("\nFinalizó la monitorización del sistema.")

# Ejecutar la monitorización (duración total de 30 segundos, con intervalos de 5 segundos)
if __name__ == "__main__":
    monitorizar_sistema(duracion=30, intervalo=5)
    import matplotlib.pyplot as plt
    import plotly.graph_objects as go

    # Función para graficar las estadísticas del sistema
    def graficar_estadisticas(tiempos, uso_cpu, uso_memoria, uso_disco):
        plt.figure(figsize=(10, 6))

        plt.subplot(3, 1, 1)
        plt.plot(tiempos, uso_cpu, label='Uso de CPU (%)', color='r')
        plt.ylabel('CPU (%)')
        plt.legend(loc='upper right')

        plt.subplot(3, 1, 2)
        plt.plot(tiempos, uso_memoria, label='Uso de Memoria (%)', color='g')
        plt.ylabel('Memoria (%)')
        plt.legend(loc='upper right')

        plt.subplot(3, 1, 3)
        plt.plot(tiempos, uso_disco, label='Uso de Disco (%)', color='b')
        plt.xlabel('Tiempo (s)')
        plt.ylabel('Disco (%)')
        plt.legend(loc='upper right')

        plt.tight_layout()
        plt.show()

    # Función para graficar las estadísticas del sistema usando Plotly
    def graficar_estadisticas_plotly(tiempos, uso_cpu, uso_memoria, uso_disco):
        fig = go.Figure()

        fig.add_trace(go.Scatter(x=tiempos, y=uso_cpu, mode='lines+markers', name='Uso de CPU (%)'))
        fig.add_trace(go.Scatter(x=tiempos, y=uso_memoria, mode='lines+markers', name='Uso de Memoria (%)'))
        fig.add_trace(go.Scatter(x=tiempos, y=uso_disco, mode='lines+markers', name='Uso de Disco (%)'))

        fig.update_layout(
            title='Estadísticas del Sistema',
            xaxis_title='Tiempo (s)',
            yaxis_title='Uso (%)',
            legend_title='Métricas',
            template='plotly_dark'
        )

        fig.show()

    # Modificar la función principal para registrar y graficar las métricas usando Plotly
    def monitorizar_sistema(duracion=30, intervalo=5):
        print("Iniciando monitorización del sistema...")
        inicio = datetime.now()

        tiempos = []
        uso_cpu = []
        uso_memoria = []
        uso_disco = []

        try:
            while (datetime.now() - inicio).seconds < duracion:
                tiempo_actual = (datetime.now() - inicio).seconds
                tiempos.append(tiempo_actual)

                uso_cpu_actual = psutil.cpu_percent(interval=1)
                uso_cpu.append(uso_cpu_actual)

                memoria = psutil.virtual_memory()
                uso_memoria.append(memoria.percent)

                disco = psutil.disk_usage('/')
                uso_disco.append(disco.percent)

                mostrar_estadisticas()
                time.sleep(intervalo)
        except KeyboardInterrupt:
            print("\nMonitorización detenida por el usuario.")
        finally:
            print("\nFinalizó la monitorización del sistema.")
            graficar_estadisticas_plotly(tiempos, uso_cpu, uso_memoria, uso_disco)
def obtener_uso_cpu():
    return psutil.cpu_percent(interval=1)

def obtener_uso_memoria():
    memoria = psutil.virtual_memory()
    return memoria.percent

def obtener_uso_disco():
    disco = psutil.disk_usage('/')
    return disco.percent
def graficar_uso_cpu_memoria_disco(cpu, memoria, disco):
    etiquetas = ['CPU', 'Memoria', 'Disco']
    valores = [cpu, memoria, disco]
    
    plt.bar(etiquetas, valores, color=['red', 'green', 'blue'])
    plt.title('Uso de Recursos del Sistema')
    plt.ylabel('Porcentaje (%)')
    plt.show()

# Función para graficar el uso de CPU, memoria y disco
def graficar_interactivo(cpu, memoria, disco):
    fig = go.Figure(data=[go.Pie(labels=['CPU', 'Memoria', 'Disco'],
                                 values=[cpu, memoria, disco],
                                 hole=0.3)])
    
    fig.update_layout(title_text="Uso de Recursos del Sistema")
    fig.show()
if __name__ == "__main__":  
    monitorizar_sistema(duracion=30, intervalo=5)
import time

def monitorizar():
    while True:
        cpu = obtener_uso_cpu()
        memoria = obtener_uso_memoria()
        disco = obtener_uso_disco()
        
        # Graficar con matplotlib
        graficar_uso_cpu_memoria_disco(cpu, memoria, disco)
        
        # Graficar con plotly (puedes elegir uno u otro)
        # graficar_interactivo(cpu, memoria, disco)
        
        time.sleep(5)  # Espera 5 segundos antes de recopilar los datos nuevamente

if __name__ == "__main__":
    monitorizar()
import matplotlib.pyplot as plt
import psutil
from matplotlib.animation import FuncAnimation

# Función para obtener los datos
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco

# Configuración del gráfico
fig, ax = plt.subplots()
etiquetas = ['CPU', 'Memoria', 'Disco']
barras = ax.bar(etiquetas, [0, 0, 0], color=['red', 'green', 'blue'])

def actualizar(frame):
    cpu, memoria, disco = obtener_datos()
    # Actualizamos las alturas de las barras
    barras[0].set_height(cpu)
    barras[1].set_height(memoria)
    barras[2].set_height(disco)
    ax.set_title(f'Uso de Recursos - CPU: {cpu}% Memoria: {memoria}% Disco: {disco}%')

# Crear la animación
ani = FuncAnimation(fig, actualizar, interval=1000)  # Se actualiza cada 1 segundo
plt.show()
import plotly.graph_objects as go
import psutil
from plotly.subplots import make_sub
import time
import plotly.graph_objects as go
import psutil
import time

# Función para obtener los datos
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco

# Inicialización de listas para almacenar datos a lo largo del tiempo
tiempo = []
cpu_vals = []
memoria_vals = []
disco_vals = []

# Gráfico interactivo con Plotly
fig = go.Figure()

# Agregar las líneas de CPU, Memoria y Disco
fig.add_trace(go.Scatter(x=tiempo, y=cpu_vals, mode='lines', name='CPU', line=dict(color='red')))
fig.add_trace(go.Scatter(x=tiempo, y=memoria_vals, mode='lines', name='Memoria', line=dict(color='green')))
fig.add_trace(go.Scatter(x=tiempo, y=disco_vals, mode='lines', name='Disco', line=dict(color='blue')))

fig.update_layout(title="Uso de Recursos del Sistema",
                  xaxis_title="Tiempo",
                  yaxis_title="Porcentaje (%)",
                  showlegend=True)

# Función para actualizar los datos y gráficos
def actualizar():
    while True:
        cpu, memoria, disco = obtener_datos()
        tiempo.append(time.time())
        cpu_vals.append(cpu)
        memoria_vals.append(memoria)
        disco_vals.append(disco)
        
        # Limitar el número de puntos en el gráfico
        if len(tiempo) > 50:
            tiempo.pop(0)
            cpu_vals.pop(0)
            memoria_vals.pop(0)
            disco_vals.pop(0)
        
        # Actualizar los datos de los trazos
        fig.data[0].x = tiempo
        fig.data[0].y = cpu_vals
        fig.data[1].x = tiempo
        fig.data[1].y = memoria_vals
        fig.data[2].x = tiempo
        fig.data[2].y = disco_vals
        
        fig.show()
        time.sleep(1)

# Ejecutamos la actualización en tiempo real
actualizar()
import matplotlib.pyplot as plt
import psutil
from matplotlib.animation import Func
import matplotlib.pyplot as plt
import psutil
from matplotlib.animation import Func
import matplotlib.pyplot as plt
import psutil
def graficar_alerta(cpu, memoria, disco):
    fig, ax = plt.subplots()
    etiquetas = ['CPU', 'Memoria', 'Disco']
    valores = [cpu, memoria, disco]
    
    # Colores de fondo dependiendo del umbral
    color_cpu = 'red' if cpu > 80 else 'green'
    color_memoria = 'red' if memoria > 80 else 'green'
    color_disco = 'red' if disco > 80 else 'green'
    
    ax.bar(etiquetas, valores, color=[color_cpu, color_memoria, color_disco])
    ax.set_title(f'Uso de Recursos - CPU: {cpu}% Memoria: {memoria}% Disco: {disco}%')
    ax.set_ylabel('Porcentaje (%)')
    plt.show()

# Para usar la función, solo pasas los datos obtenidos:
cpu, memoria, disco = obtener_datos()
graficar_alerta(cpu, memoria, disco)
import plotly.graph_objects as go
import psutil
import time
import matplotlib.pyplot as plt
import psutil
import time
from matplotlib.animation import FuncAnimation

# Función para obtener los datos del sistema
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco

# Configuración inicial del gráfico
fig, ax = plt.subplots()
etiquetas = ['CPU', 'Memoria', 'Disco']
barras = ax.bar(etiquetas, [0, 0, 0], color=['red', 'green', 'blue'])

# Variable para almacenar el tiempo de inicio
tiempo_inicio = time.time()

def actualizar(frame):
    cpu, memoria, disco = obtener_datos()
    
    # Actualizamos las alturas de las barras
    barras[0].set_height(cpu)
    barras[1].set_height(memoria)
    barras[2].set_height(disco)
    
    # Calculamos el tiempo transcurrido
    tiempo_transcurrido = int(time.time() - tiempo_inicio)
    minutos = tiempo_transcurrido // 60
    segundos = tiempo_transcurrido % 60
    
    # Actualizamos el título con el tiempo
    ax.set_title(f'Uso de Recursos - Tiempo: {minutos:02}:{segundos:02} - CPU: {cpu}% Memoria: {memoria}% Disco: {disco}%')

# Crear la animación
ani = FuncAnimation(fig, actualizar, interval=1000)  # Actualiza cada 1 segundo
plt.show()
import plotly.graph_objects as go


# Función para obtener los datos del sistema
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco 
import plotly.graph_objects as go
import psutil
import time

# Función para obtener los datos del sistema
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco

# Inicialización de listas para almacenar los datos históricos
tiempo = []
cpu_vals = []
memoria_vals = []
disco_vals = []

# Inicialización del gráfico con Plotly
fig = go.Figure()

# Agregar líneas de CPU, Memoria y Disco
fig.add_trace(go.Scatter(x=tiempo, y=cpu_vals, mode='lines', name='CPU', line=dict(color='red')))
fig.add_trace(go.Scatter(x=tiempo, y=memoria_vals, mode='lines', name='Memoria', line=dict(color='green')))
fig.add_trace(go.Scatter(x=tiempo, y=disco_vals, mode='lines', name='Disco', line=dict(color='blue')))

fig.update_layout(
    title="Uso de Recursos del Sistema",
    xaxis_title="Tiempo",
    yaxis_title="Porcentaje (%)",
    showlegend=True
)

# Variable para almacenar el tiempo de inicio
tiempo_inicio = time.time()

# Función para actualizar los datos y el gráfico
def actualizar():
    while True:
        # Obtener los nuevos datos
        cpu, memoria, disco = obtener_datos()
        
        # Añadir los datos a las listas
        tiempo.append(time.time())
        cpu_vals.append(cpu)
        memoria_vals.append(memoria)
        disco_vals.append(disco)
        
        # Limitar la cantidad de datos en el gráfico para que no sea muy pesado
        if len(tiempo) > 50:
            tiempo.pop(0)
            cpu_vals.pop(0)
            memoria_vals.pop(0)
            disco_vals.pop(0)
        
        # Actualizar las líneas de los gráficos
        fig.data[0].x = tiempo
        fig.data[0].y = cpu_vals
        fig.data[1].x = tiempo
        fig.data[1].y = memoria_vals
        fig.data[2].x = tiempo
        fig.data[2].y = disco_vals

        # Calcular el tiempo transcurrido
        tiempo_transcurrido = int(time.time() - tiempo_inicio)
        minutos = tiempo_transcurrido // 60
        segundos = tiempo_transcurrido % 60

        # Actualizar el título con el tiempo
        fig.update_layout(
            title=f'Uso de Recursos del Sistema - Tiempo: {minutos:02}:{segundos:02}'
        )
        
        # Mostrar el gráfico
        fig.show()
        
        # Pausar 1 segundo antes de actualizar
        time.sleep(1)

# Ejecutar la actualización en tiempo real
actualizar()
import matplotlib.pyplot as plt
import psutil
import time

# Función para obtener los datos del sistema
def obtener_datos():
    cpu = psutil.cpu_percent(interval=1)
    memoria = psutil.virtual_memory().percent
    disco = psutil.disk_usage('/').percent
    return cpu, memoria, disco
