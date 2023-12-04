import machine
import time

# Configura el pin GPIO al que está conectado el servo
servo_pin = machine.Pin(4)  # Cambia el número del pin según tu conexión

# Configura el servo
servo = machine.PWM(servo_pin, freq=50)  # Frecuencia de 50Hz para servos

# Función para mover el servo a una posición específica
def move_servo(position):
    # Convierte la posición deseada (0-180 grados) al valor de ciclo de trabajo PWM
    duty = int(((position / 180.0) * 65) + 40)  # Ajusta los valores según sea necesario
    
    # Establece el valor de ciclo de trabajo PWM
    servo.duty(duty)

# Define los números de pin para los sensores
metal = machine.Pin(12, machine.Pin.IN, machine.Pin.PULL_UP)
plastic = machine.Pin(5, machine.Pin.IN, machine.Pin.PULL_UP)

while True:
    # Lee el estado de los sensores
    metal_estado = metal.value()
    plastic_estado = plastic.value()

    if metal_estado == 0:
        print("Metal detectado")
        move_servo(180)  # Mueve el servo a 180 grados cuando se detecta metal
        time.sleep(2)
        move_servo(30)  # Mueve el servo a 90 grados cuando no se detecta metal ni plástico
    elif plastic_estado == 0:
        print("Plástico detectado")
    else:
        print("no hay envase")

    time.sleep(0.1)  # Espera un corto período para evitar lecturas rápidas
