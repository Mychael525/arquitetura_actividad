from neopixel import NeoPixel
from time import sleep
from machine import Pin, ADC, PWM  # Importar la clase PWM para el servo

# Configuración del motor (servo PWM)
servo = PWM(Pin(13), freq=50)  # Pin 13 para el servo
y = ADC(Pin(34))  # Pin analógico para el potenciómetro (puedes cambiar el pin si es necesario)
y.width(ADC.WIDTH_10BIT)  # Configuración de 10 bits (valores de 0 a 1023)
y.atten(ADC.ATTN_0DB)  # Configuración de la atenuación para el rango de 0 a 3.3V

# Configuración de los LEDs NeoPixel
num_pixels = 16
pixels = NeoPixel(Pin(15), num_pixels)

# Colores del arcoíris
rainbow = [
    (114, 13, 0), (102, 25, 0), (90, 37, 0), (78, 49, 0),
    (66, 61, 0), (54, 73, 0), (42, 85, 0), (30, 97, 0), (18, 109, 0),
    (6, 121, 0), (0, 122, 5), (0, 110, 17), (0, 98, 29), (0, 86, 41), (42, 85, 0)
]

# Posiciones del ala
left_pos = 0    # Ángulo para la izquierda
right_pos = 180  # Ángulo para la derecha

def set_servo(angle):
    """ Mueve el servo a un ángulo específico """
    duty = int(40 + (angle / 180) * 80)  # Ajuste de duty para ángulos de servo
    servo.duty(duty)

def move_led(direction, delay_time):
    """ Mueve la luz en la dirección especificada usando colores del arcoíris """
    if direction == "right":
        for i in range(num_pixels):
            # Enciende el LED en la posición actual con color del arcoíris
            pixels[i] = rainbow[i % len(rainbow)]
            # Enciende el LED blanco y los LEDs adyacentes
            pixels[(i + 1) % num_pixels] = (255, 255, 255)  # LED blanco
            pixels[(i + 2) % num_pixels] = rainbow[(i + 2) % len(rainbow)]  # Color del arcoíris en el segundo LED adyacente
            pixels.write()
            sleep(delay_time)
    elif direction == "left":
        for i in range(num_pixels - 1, -1, -1):
            # Enciende el LED en la posición actual con color del arcoíris
            pixels[i] = rainbow[i % len(rainbow)]
            # Enciende el LED blanco y los LEDs adyacentes
            pixels[(i - 1) % num_pixels] = (255, 255, 255)  # LED blanco
            pixels[(i - 2) % num_pixels] = rainbow[(i - 2) % len(rainbow)]  # Color del arcoíris en el segundo LED adyacente
            pixels.write()
            sleep(delay_time)

# Bucle automático
while True:
    # Lee el valor del potenciómetro (0 a 1023)
    pot_value = y.read()

    # Calculamos el delay en función del valor del potenciómetro
    # Mapeamos el valor a un rango de tiempos de espera más pequeño para la izquierda y más grande para la derecha
    delay_time = 0.1 - (pot_value / 1023.0) * 0.09  # Mueve más rápido cuando se acerca a 0 (izquierda) y más lento cuando se acerca a 1023 (derecha)

    # Aseguramos que el delay_time no sea menor que 0.01
    delay_time = max(delay_time, 0.01)

    # Movemos a la izquierda
    print(f"Moviendo a la izquierda, potenciómetro: {pot_value}, delay: {delay_time:.4f}")
    set_servo(left_pos)  # Mueve el ala a la izquierda
    move_led("left", delay_time)  # Mueve la luz a la izquierda
    sleep(0.5)

    # Movemos a la derecha
    print(f"Moviendo a la derecha, potenciómetro: {pot_value}, delay: {delay_time:.4f}")
    set_servo(right_pos)  # Mueve el ala a la derecha
    move_led("right", delay_time)  # Mueve la luz a la derecha
    sleep(0.5)
