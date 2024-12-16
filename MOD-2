from sqlalchemy import create_engine, Column, Integer, String, Text
from sqlalchemy.orm import sessionmaker, declarative_base

Base = declarative_base()
engine = create_engine('sqlite:///librorecetas.db', echo=False, future=True)
Session = sessionmaker(bind=engine, autoflush=False, future=True)


class Receta(Base):
    __tablename__ = 'librorecetas'

    id = Column(Integer, primary_key=True, autoincrement=True)
    nombre = Column(String(100), nullable=False)  # Eliminado el unique
    ingredientes = Column(Text, nullable=False)
    pasos = Column(Text, nullable=False)

Base.metadata.create_all(engine)


def crear_receta(nombre, ingredientes, pasos):
    with Session() as session:
        nueva_receta = Receta(nombre=nombre, ingredientes=ingredientes, pasos=pasos)
        session.add(nueva_receta)
        session.commit()
        print("Receta creada exitosamente.")

def leer_recetas():
    with Session() as session:
        return session.query(Receta).all()

def actualizar_receta(nombre, nuevo_nombre=None, nuevos_ingredientes=None, nuevos_pasos=None):
    with Session() as session:
        receta = session.query(Receta).filter_by(nombre=nombre).first()
        if receta:
            receta.nombre = nuevo_nombre or receta.nombre
            receta.ingredientes = nuevos_ingredientes or receta.ingredientes
            receta.pasos = nuevos_pasos or receta.pasos
            session.commit()
            print("Receta actualizada exitosamente.")
        else:
            print("Error: No se encontró la receta especificada.")

def eliminar_receta(nombre):
    with Session() as session:
        receta = session.query(Receta).filter_by(nombre=nombre).first()
        if receta:
            session.delete(receta)
            session.commit()
            print("Receta eliminada exitosamente.")
        else:
            print("Error: No se encontró la receta especificada.")


def receta_menu():
    while True:
        print("\nMenu de Recetas")
        print("1. Crear Receta")
        print("2. Ver Recetas")
        print("3. Actualizar Receta")
        print("4. Eliminar Receta")
        print("5. Salir")
        opcion = input("OPCION :: ")

        if opcion == '1':
            nombre = input("Nombre de la receta: ")
            ingredientes = input("Ingredientes (separados por comas): ")
            pasos = input("Pasos (separados por comas): ")
            crear_receta(nombre, ingredientes, pasos)
        elif opcion == '2':
            recetas = leer_recetas()
            if recetas:
                print("\nRecetas registradas:")
                for receta in recetas:
                    print(f"Nombre: {receta.nombre}, Ingredientes: {receta.ingredientes}, Pasos: {receta.pasos}")
            else:
                print("No hay recetas registradas.")
        elif opcion == '3':
            nombre = input("Nombre de la receta a actualizar: ")
            nuevo_nombre = input("Nuevo nombre (dejar vacío para no cambiar): ")
            nuevos_ingredientes = input("Nuevos ingredientes (dejar vacío para no cambiar): ")
            nuevos_pasos = input("Nuevos pasos (dejar vacío para no cambiar): ")
            actualizar_receta(nombre, nuevo_nombre, nuevos_ingredientes, nuevos_pasos)
        elif opcion == '4':
            nombre = input("Nombre de la receta a eliminar: ")
            eliminar_receta(nombre)
        elif opcion == '5':
            print("Saliendo del programa.")
            break
        else:
            print("ERR:: Opción inválida")

if __name__ == "__main__":
    receta_menu()
    
