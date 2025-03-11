import tkinter as tk
from tkinter import messagebox
from tkinter import font
from datetime import datetime

# Aquí va la ruta del archivo
Ruta = "C:/Quinto/archivo.txt"

def agregar_invitado(nombre, automovil, regalo):
    """
    Función para agregar un nombre de invitado al archivo.
    """
    try:
        with open(Ruta, "a") as archivo:
            archivo.write(f"{nombre},{automovil},{regalo}\n")
        messagebox.showinfo("Éxito", f"El invitado {nombre} ha sido agregado correctamente.")
    except Exception as e:
        messagebox.showerror("Error", f"Ocurrió un error: {e}")

def mostrar_invitados():
    """
    Función para mostrar todos los nombres de los invitados almacenados en el archivo.
    """
    try:
        with open(Ruta, "r") as archivo:
            invitados = archivo.readlines()
            if invitados:
                invitados_lista = "\n".join([invitado.strip() for invitado in invitados])
                messagebox.showinfo("Lista de Invitados", f"Lista de invitados:\n{invitados_lista}")
            else:
                messagebox.showinfo("Lista Vacía", "No hay invitados en la lista.")
    except FileNotFoundError:
        messagebox.showerror("Error", "El archivo no existe. No se pueden mostrar los invitados.")
    except Exception as e:
        messagebox.showerror("Error", f"Ocurrió un error: {e}")

def eliminar_invitado(nombre):
    """
    Función para eliminar un nombre de invitado del archivo.
    """
    try:
        with open(Ruta, "r") as archivo:
            invitados = archivo.readlines()

        invitados_actualizados = [invitado for invitado in invitados if invitado.split(',')[0].strip() != nombre]

        if len(invitados_actualizados) == len(invitados):
            messagebox.showinfo("No encontrado", f"No se encontró al invitado {nombre} en la lista.")
        else:
            with open(Ruta, "w") as archivo:
                archivo.writelines(invitados_actualizados)
            messagebox.showinfo("Éxito", f"El invitado {nombre} ha sido eliminado correctamente.")
    except FileNotFoundError:
        messagebox.showerror("Error", "El archivo no existe. No se pueden eliminar invitados.")
    except Exception as e:
        messagebox.showerror("Error", f"Ocurrió un error: {e}")

def agregar_invitado_gui():
    nombre = entry_nombre.get()
    automovil = entry_automovil.get().strip().lower() == 'sí'
    regalo = entry_regalo.get()
    
    if nombre:
        agregar_invitado(nombre, automovil, regalo)
        entry_nombre.delete(0, tk.END)
        entry_automovil.delete(0, tk.END)
        entry_regalo.delete(0, tk.END)
    else:
        messagebox.showwarning("Entrada Vacía", "Por favor ingresa un nombre.")

def eliminar_inv_gui():
    nombre = entry_nombre.get()
    if nombre:
        eliminar_invitado(nombre)
        entry_nombre.delete(0, tk.END)
    else:
        messagebox.showwarning("Entrada Vacía", "Por favor ingresa un nombre para eliminar.")

def mostrar_inv_gui():
    mostrar_invitados()

# Función para calcular la cuota total y contar invitados
def mostrar_resumen_gui():
    try:
        with open(Ruta, "r") as archivo:
            invitados = archivo.readlines()
            total_invitados = len(invitados)
            total_cuota = total_invitados * 100
            messagebox.showinfo("Resumen", f"Total de invitados: {total_invitados}\nTotal cuota: Q{total_cuota:.2f}")
    except FileNotFoundError:
        messagebox.showerror("Error", "El archivo no existe. No se pueden calcular los datos.")
    except Exception as e:
        messagebox.showerror("Error", f"Ocurrió un error: {e}")

# Configuración de la ventana principal
ventana = tk.Tk()
ventana.title("Gestión de Invitados a la Fiesta")
ventana.geometry("600x700")
ventana.config(bg="#FFD1DC")  # Rosa claro para el fondo

# Frame para crear un margen blanco alrededor del contenido
frame = tk.Frame(ventana, bg="white", padx=20, pady=20)
frame.pack(fill="both", expand=True)

# Fuente personalizada
fuente_fiesta = font.Font(family="Helvetica", size=18, weight="bold")

# Etiqueta principal
label = tk.Label(frame, text="Gestión de Invitados a la Fiesta", font=fuente_fiesta, fg="#800000", bg="white")
label.pack(pady=20)

# Caja de entrada para el nombre del invitado
entry_nombre = tk.Entry(frame, font=("Arial", 14), width=30)
entry_nombre.pack(pady=10)
entry_nombre.insert(0, "Nombre del invitado")

# Caja de entrada para el automóvil
entry_automovil = tk.Entry(frame, font=("Arial", 14), width=30)
entry_automovil.pack(pady=10)
entry_automovil.insert(0, "¿Llevará automóvil? (sí/no)")

# Caja de entrada para regalo o dinero
entry_regalo = tk.Entry(frame, font=("Arial", 14), width=30)
entry_regalo.pack(pady=10)
entry_regalo.insert(0, "¿Regalo o dinero? (regalo/dinero)")

# Botones para las funciones
btn_agregar = tk.Button(frame, text="Agregar Invitado", font=("Arial", 14), command=agregar_invitado_gui, bg="#800000", fg="white")
btn_agregar.pack(pady=10)

btn_mostrar = tk.Button(frame, text="Mostrar Invitados", font=("Arial", 14), command=mostrar_inv_gui, bg="#800000", fg="white")
btn_mostrar.pack(pady=10)

btn_eliminar = tk.Button(frame, text="Eliminar Invitado", font=("Arial", 14), command=eliminar_inv_gui, bg="#800000", fg="white")
btn_eliminar.pack(pady=10)

btn_resumen = tk.Button(frame, text="Resumen de Invitados", font=("Arial", 14), command=mostrar_resumen_gui, bg="#800000", fg="white")
btn_resumen.pack(pady=10)

# Botón para salir
btn_salir = tk.Button(frame, text="Salir", font=("Arial", 14), command=ventana.quit, bg="red", fg="white")
btn_salir.pack(pady=20)

# Ejecutar la ventana principal
ventana.mainloop()
