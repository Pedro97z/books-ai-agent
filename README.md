🤖 Agente de libros con LangChain y GPT-4o-mini

Este proyecto presenta la implementación de un agente conversacional capaz de interactuar con usuarios a través de Google Colab, utilizando GPT-4o-mini, memoria temporal con checkpointer, conexión a APIs mediante herramientas y un flujo conversacional basado en el patrón ReAct de LangChain.


1. Descripción del agente

El agente está diseñado para ayudar en la búsqueda de libros en librerías locales y recomendar ejemplares de acuerdo a las preferencias del usuario.
El modelo utilizado es gpt-4o-mini, esta integrado con el método ReAct (create_react_agent) de LangChain el cual contiene herramientas personalizadas y externas, y cuenta con memoria por sesión vía thread_id.

2. Diagrama de arquitectura

El diagrama muestra las características del agente y el flujo de interacción con el usuario. El agente basa su entendimiento del lenguaje natural en el modelo GPT4o-mini. El Prompt System guía el comportamiento del agente para un correcto uso de las herramientas que posee tanto personalizadas como de terceros. Asimismo, el método ReAct permite al agente seguir la planificación, ejecución y observación de tareas antes de entregar la respuesta final al usuario.

<img width="2013" height="1006" alt="Proyecto Modulo 2 - Pedro Zuñiga" src="https://github.com/user-attachments/assets/45dd1f81-860f-45f3-9314-9aa9dea9b310" />

2. Descripción de herramientas y funciones

Herramienta 1: get_libros_collection

Esta herramienta personalizada permite obtener toda la colección de documentos de una base de datos, almacenada en un cluster de MongoDB, predefinida. La función retorna un objeto Collection del cual se pueden extraer los datos de cada documento.

Herramienta 2: libreriasLibro

Esta herramienta personalizada permite obtener una lista de librerias en base al título de un libro. La función recupera los documentos de la colección y retorna una lista con los nombres de librerias asociadas al libro.

Herramienta 3: stockLibro

Esta herramienta personalizada permite obtener una lista con un diccionario de librerias y stock en base al título de un libro. La función recupera los documentos de la colección y añade las librerias y el stock de manera ordenada en la lista.

Herramienta 4: descripcionLibro

Esta herramienta de terceros permite obtener la descripción de un libro a través de una solicitud al API de Google Books. La función obtiene la descripción en base al libro.

https://www.googleapis.com/books/v1/volumes?q=" + titulo + "&key=" + googlecloud_api_key

Herramienta 5: resenasLibro

Esta herramienta de terceros permite obtener una lista con 5 reseñas de un libro a través de una solicitud al API de Hardcover. Primero, la función obtiene el slug (código del libro en Hardcover) mediante una llamada a la función slugLibro la cual recibe como entrada el título del libro. Luego, arma una query para obtener todas las reseñas en base al libro. Finalmente, recupera las 5 primeras reseñas del libro y retorna una lista con estas reseñas.

query = f"""
    query {{
        search(
            query: "{titulo}",
            query_type: "books",
            per_page: 1,
            page: 1
        ){{
            results
        }}
    }}
    """


query = f"""
    query {{
        user_books(
            where: {{
                book: {{
                    slug: {{
                        _eq: "{slug_libro}"
                    }}
                }}
            }}
        ){{
            review
        }}
    }}
    """


Herramienta 6: traducir_es_tool

Esta herramienta personalizada permite traducir un texto en cualquier idioma a español. Se utiliza para traducir las reseñas de libros que, usualmente, se obtienen en inglés.

Herramienta 7: tavily_web_search

Esta herramienta de terceros permite obtener información general sobre el título de algún libro. Se utiliza para brindar detalles del libro.

Asistente conversacional basado en GPT-4o-mini e integrado con LangChain y diferentes APIs para ayudar en la búsqueda de libros en librerías locales y recomendar ejemplares de acuerdo a las preferencias del usuario. 

