import tkinter as tk
from tkinter import messagebox
import time
from playsound import playsound
import threading # Para reproducir música en segundo plano

class PastelDeCumpleaños:
    def __init__(self, master, nombre="SAÚL", mensaje="¡Feliz cumpleaños, amigo Saúl!", musica_path="feliz_cumpleanos.mp3"):
        self.master = master
        master.title("Feliz Cumpleaños")
        master.geometry("800x600")
        master.resizable(False, False)
        
        self.canvas = tk.Canvas(master, bg="#FFDAB9", highlightthickness=0)
        self.canvas.pack(fill=tk.BOTH, expand=True)

        self.nombre = nombre.upper()
        self.mensaje = mensaje
        self.musica_path = musica_path
        
        self.capas_info = [] # Almacenar rectángulos y texto de las capas
        self.velas_info = [] # Almacenar objetos de velas y llamas
        self.animacion_paso = 0 # Para controlar la animación de construcción del pastel

        self.colores_pastel = ["#90EE90", "#ADD8E6", "#FFD700", "#FFC0CB"] # Verde, Azul, Oro, Rosa (para 4 capas)
        
        self.dibujar_mensaje()
        self.iniciar_animacion_pastel()
        self.reproducir_musica()

    def dibujar_mensaje(self):
        self.canvas.create_text(self.canvas.winfo_width() / 2, 50,
                                text=self.mensaje, 
                                font=("Georgia", 30, "italic bold"), 
                                fill="#8A2BE2",
                                tags="mensaje")
        
    def iniciar_animacion_pastel(self):
        # Limpiar cualquier pastel existente
        self.canvas.delete("pastel")
        self.canvas.delete("pastel_letra")
        self.canvas.delete("vela")
        self.canvas.delete("llama")
        self.capas_info = []
        self.velas_info = []
        self.animacion_paso = 0
        self._animar_creacion_capa() # Iniciar la animación de creación

    def _animar_creacion_capa(self):
        base_x = self.canvas.winfo_width() / 2
        base_y = self.canvas.winfo_height() - 50
        ancho_base_inicial = 230 # Ancho de la capa más baja (para 'S')
        altura_capa = 40
        espacio_entre_capas = 5

        num_letras = len(self.nombre)
        
        if self.animacion_paso < num_letras:
            i = self.animacion_paso
            letra = self.nombre[num_letras - 1 - i] # De la 'S' a la 'L'
            
            ancho_capa = ancho_base_inicial - (i * 35) 
            
            x0 = base_x - (ancho_capa / 2)
            y0 = base_y - (altura_capa + espacio_entre_capas) * (i + 1)
            x1 = base_x + (ancho_capa / 2)
            y1 = base_y - (altura_capa + espacio_entre_capas) * i - espacio_entre_capas

            color = self.colores_pastel[i % len(self.colores_pastel)]
            
            # Capa del pastel
            capa_rect = self.canvas.create_rectangle(x0, y0, x1, y1, 
                                                     fill=color, outline="#8B4513", width=2,
                                                     tags="pastel")
            self.canvas.create_line(x0, y0 + 10, x1, y0 + 10, fill="white", width=2, dash=(5, 3), tags="pastel")
            self.canvas.create_line(x0, y1 - 10, x1, y1 - 10, fill="white", width=2, dash=(5, 3), tags="pastel")

            # Letra en la capa
            letra_id = self.canvas.create_text(base_x, y0 + altura_capa / 2, 
                                                text=letra, font=("Comic Sans MS", 24, "bold"), 
                                                fill="#6A5ACD", tags="pastel_letra")
            
            self.capas_info.append({"rect": capa_rect, "text": letra_id, "letra": letra})
            
            self.animacion_paso += 1
            self.master.after(500, self._animar_creacion_capa) # Siguiente capa después de 0.5 segundos
        else:
            # Una vez que todas las capas están dibujadas, dibujar el plato y las velas
            self.dibujar_plato()
            self.dibujar_velas()
            self.animar_encendido_velas(0) # Iniciar el encendido de las velas

    def dibujar_plato(self):
        # El plato debe estar debajo de la capa inferior
        if not self.capas_info: return
        
        # Obtener la posición de la capa más baja
        bbox_capa_inferior = self.canvas.bbox(self.capas_info[0]["rect"])
        base_x = (bbox_capa_inferior[0] + bbox_capa_inferior[2]) / 2
        base_y = bbox_capa_inferior[3] + 20 # Un poco más abajo que la base de la primera capa
        ancho_plato = (bbox_capa_inferior[2] - bbox_capa_inferior[0]) + 60 # Más ancho que la capa inferior
        
        plato_alto = 20
        plato_x0 = base_x - (ancho_plato / 2)
        plato_y0 = base_y - plato_alto / 2
        plato_x1 = base_x + (ancho_plato / 2)
        plato_y1 = base_y + plato_alto / 2
        self.canvas.create_oval(plato_x0, plato_y0, plato_x1, plato_y1, 
                                fill="#D3D3D3", outline="#A9A9A9", width=2, tags="pastel")


    def dibujar_velas(self):
        if not self.capas_info:
            return

        # La capa más superior es la última en la lista (index -1)
        bbox_capa_superior = self.canvas.bbox(self.capas_info[-1]["rect"]) 
        cx_capa_superior = (bbox_capa_superior[0] + bbox_capa_superior[2]) / 2
        cy_capa_superior = bbox_capa_superior[1]

        num_velas = len(self.nombre) # Una vela por letra
        espacio_entre_velas = 30
        
        start_x = cx_capa_superior - ((num_velas - 1) * espacio_entre_velas / 2)
        
        for i in range(num_velas):
            vela_x = start_x + (i * espacio_entre_velas)
            vela_y_top = cy_capa_superior - 30 
            vela_y_bottom = vela_y_top + 20

            vela_id = self.canvas.create_rectangle(vela_x - 3, vela_y_top, vela_x + 3, vela_y_bottom, 
                                                fill="#FF4500", outline="brown", tags="vela")
            
            mecha_id = self.canvas.create_line(vela_x, vela_y_top, vela_x, vela_y_top - 5, 
                                            fill="black", width=1, tags="vela")

            # Llama inicialmente invisible
            llama_id = self.canvas.create_oval(vela_x - 5, vela_y_top - 15, vela_x + 5, vela_y_top - 5,
                                            fill="", outline="", tags="llama")
            
            self.velas_info.append({"vela": vela_id, "mecha": mecha_id, "llama": llama_id, "encendida": False})

    def animar_encendido_velas(self, index):
        if index < len(self.velas_info):
            vela_data = self.velas_info[index]
            llama_id = vela_data["llama"]
            
            self.canvas.itemconfig(llama_id, fill="yellow", outline="orange")
            vela_data["encendida"] = True
            
            # Iniciar el parpadeo de esta vela
            self._parpadear_llama(index)
            
            self.master.after(300, self.animar_encendido_velas, index + 1) # Encender la siguiente
        else:
            # Todas las velas encendidas, empezar el confeti
            self.dibujar_confeti()


    def _parpadear_llama(self, index):
        if index < len(self.velas_info):
            vela_data = self.velas_info[index]
            llama_id = vela_data["llama"]
            
            if vela_data["encendida"]:
                current_fill = self.canvas.itemcget(llama_id, "fill")
                if current_fill == "yellow":
                    self.canvas.itemconfig(llama_id, fill="#FF6600", outline="#CC3300")
                else:
                    self.canvas.itemconfig(llama_id, fill="yellow", outline="orange")
                
                self.master.after(200, self._parpadear_llama, index) # Seguir parpadeando
            
    def dibujar_confeti(self):
        for _ in range(30):
            x = self.canvas.winfo_width() * (0.1 + 0.8 * time.time() % 1)
            y = self.canvas.winfo_height() * (0.1 + 0.8 * time.time() % 1)
            color = ["red", "blue", "green", "purple", "orange", "pink"][int(time.time() * 10) % 6]
            self.canvas.create_oval(x, y, x + 8, y + 8, fill=color, outline=color, tags="confeti")
        
        self.master.after(100, self.animar_confeti)

    def animar_confeti(self):
        # Mover confeti hacia abajo y reaparecer
        for confeti_item in self.canvas.find_withtag("confeti"):
            self.canvas.move(confeti_item, 0, 5) # Mover 5px hacia abajo
            
            x0, y0, x1, y1 = self.canvas.coords(confeti_item)
            if y0 > self.canvas.winfo_height():
                # Reaparecer arriba
                new_x = self.canvas.winfo_width() * (0.1 + 0.8 * time.time() % 1)
                self.canvas.coords(confeti_item, new_x, -10, new_x + 8, -2)

        self.master.after(50, self.animar_confeti) # Actualizar rápidamente


    def reproducir_musica(self):
        try:
            # playsound bloquea el hilo principal, así que lo ejecutamos en un hilo separado
            threading.Thread(target=playsound, args=(self.musica_path,)).start()
        except Exception as e:
            messagebox.showerror("Error de Música", f"No se pudo reproducir la música: {e}\nAsegúrate de tener '{self.musica_path}' en la misma carpeta o instala 'playsound' (`pip install playsound`).")

# Asegurarse de que el script se ejecuta correctamente al inicio
if __name__ == "__main__":
    # Necesitas un archivo MP3 llamado 'feliz_cumpleanos.mp3' en la misma carpeta
    # o cambia la ruta a la ubicación de tu archivo.
    # Puedes descargar uno gratuito desde YouTube Audio Library o buscar "happy birthday mp3"
    
    root = tk.Tk()
    # Retrasar la creación del objeto PastelDeCumpleaños hasta que la ventana esté lista
    # para asegurar que self.canvas.winfo_width() y self.canvas.winfo_height() tengan valores correctos.
    root.update_idletasks() 
    mi_pastel = PastelDeCumpleaños(root)
    root.mainloop()
