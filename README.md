# inventario_uea
Proyecto de Inventario desarrollado en Python como parte de la práctica académica en la asignatura de Programación. El sistema permite registrar, actualizar, eliminar y consultar productos, implementando persistencia de datos en un archivo JSON. Este repositorio contiene el código fuente y los archivos necesarios para ejecutar la aplicación.
import json

class Producto:
    def _init_(self, id_producto, nombre, cantidad, precio):
        self.id_producto = id_producto
        self.nombre = nombre
        self.cantidad = cantidad
        self.precio = precio

    def _str_(self):
        return f"{self.id_producto} - {self.nombre} - Cantidad: {self.cantidad}, Precio: {self.precio}"

class Inventario:
    def _init_(self):
        self.productos = {}
        self.cargar_datos()

    def agregar_producto(self, producto):
        if producto.id_producto in self.productos:
            print("El ID ya existe en el inventario.")
        else:
            self.productos[producto.id_producto] = producto
            print("Producto agregado correctamente.")
            self.guardar_datos()

    def eliminar_producto(self, id_producto):
        if id_producto in self.productos:
            del self.productos[id_producto]
            print("Producto eliminado.")
            self.guardar_datos()
        else:
            print("No se encontró el producto con ese ID.")

    def actualizar_producto(self, id_producto, cantidad=None, precio=None):
        if id_producto in self.productos:
            if cantidad is not None:
                self.productos[id_producto].cantidad = cantidad
            if precio is not None:
                self.productos[id_producto].precio = precio
            print("Producto actualizado.")
            self.guardar_datos()
        else:
            print("No se encontró el producto con ese ID.")

    def buscar_producto(self, id_producto):
        if id_producto in self.productos:
            print(self.productos[id_producto])
        else:
            print("No se encontró el producto.")

    def mostrar_productos(self):
        if self.productos:
            for producto in self.productos.values():
                print(producto)
        else:
            print("El inventario está vacío.")

    def guardar_datos(self):
        datos = {id_p: vars(prod) for id_p, prod in self.productos.items()}
        with open("inventario.json", "w") as f:
            json.dump(datos, f)

    def cargar_datos(self):
        try:
            with open("inventario.json", "r") as f:
                datos = json.load(f)
                for id_p, prod in datos.items():
                    self.productos[id_p] = Producto(**prod)
        except FileNotFoundError:
            pass

def menu():
    inventario = Inventario()
    while True:
        print("\n--- Menú Inventario ---")
        print("1. Agregar producto")
        print("2. Eliminar producto")
        print("3. Actualizar producto")
        print("4. Buscar producto")
        print("5. Mostrar productos")
        print("6. Salir")

        opcion = input("Elige una opción: ")

        if opcion == "1":
            id_p = input("ID: ")
            nombre = input("Nombre: ")
            cantidad = int(input("Cantidad: "))
            precio = float(input("Precio: "))
            producto = Producto(id_p, nombre, cantidad, precio)
            inventario.agregar_producto(producto)

        elif opcion == "2":
            id_p = input("ID del producto a eliminar: ")
            inventario.eliminar_producto(id_p)

        elif opcion == "3":
            id_p = input("ID del producto a actualizar: ")
            cantidad = input("Nueva cantidad (dejar vacío si no cambia): ")
            precio = input("Nuevo precio (dejar vacío si no cambia): ")
            inventario.actualizar_producto(
                id_p,
                int(cantidad) if cantidad else None,
                float(precio) if precio else None
            )

        elif opcion == "4":
            id_p = input("ID del producto a buscar: ")
            inventario.buscar_producto(id_p)

        elif opcion == "5":
            inventario.mostrar_productos()

        elif opcion == "6":
            print("Saliendo del programa...")
            break

        else:
            print("Opción no válida.")

if _name_ == "_main_":
    menu()
