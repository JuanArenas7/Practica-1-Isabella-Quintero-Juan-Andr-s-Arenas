# Practica-1-Isabella-Quintero-Juan-Andr-s-Arenas
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <string>

using namespace std;

class Event {
public:
    int data;
    string scientist;
    string eventType;
    Event* next;
    Event* prev;

    Event(int data, string scientist) {
        this->data = data;
        this->scientist = scientist;
        this->eventType = "";
        this->next = nullptr;
        this->prev = nullptr;
    }

    int getData() {
        return data;
    }

    string getScientist() {
        return scientist;
    }

    string getEventType() {
        return eventType;
    }

    void setEventType(string eventType) {
        this->eventType = eventType;
    }

    void setScientist(string scientist) {
        this->scientist = scientist;
    }

    void displayEvent() {
        cout << "[" << data << "|" << scientist << "|" << eventType << "]" << endl;
    }
};

class Nodo {
public:
    Nodo* prev;
    Event* event;
    Nodo* next;

    Nodo(Event* event) {
        this->prev = nullptr;
        this->event = event;
        this->next = nullptr;
    }

    Nodo(Event* event, Nodo* prev, Nodo* next) {
        this->prev = prev;
        this->event = event;
        this->next = next;
    }

    Event* getEvent() {
        return event;
    }

    Nodo* getNext() {
        return next;
    }

    Nodo* getPrev() {
        return prev;
    }

    void imprimirSiguiente() {
        cout << this->next->getEvent()->getData() << endl;
    }
};

class ListaDobleEnlazada {
public:
    Nodo* cabeza;
    Nodo* cola;
    Event* eventoA;
    Event* eventoB;
    Event* eventoC;

    ListaDobleEnlazada() {
        cabeza = nullptr;
        cola = nullptr;
        eventoA = nullptr;
        eventoB = nullptr;
        eventoC = nullptr;
    }

    bool vacio() {
        return cabeza == nullptr;
    }

    bool sonCoprimos(int a, int b) {
        int maxVal = max(a, b);
        int minVal = min(a, b);
        int resultado = 0;

        do {
            resultado = minVal;
            minVal = maxVal % minVal;
            maxVal = resultado;
        } while (minVal != 0);

        return maxVal == 1;
    }

    bool esPrimo(int numero) {
        if (numero == 1) {
            return false;
        }

        for (int i = 2; i < numero; i++) {
            if (numero % i == 0) {
                return false;
            }
        }

        return true;
    }

    void empezar() {
        int contadorEventoA = 0;
        int contadorEventoB = 0;
        int contadorEventoC = 0;
        int contadorNodos = 0;
        Nodo* ultimoNodo = nullptr;

        srand(time(nullptr));

        for (int i = 1; i <= 22; i++) {
            if (!vacio()) {
                contadorNodos++;
                int dato = rand() % 100 + 1;
                string cientifico = getCientifico();

                Event* nuevoEvento = new Event(dato, cientifico);

                bool datob = esPrimo(dato);

                if (datob && contadorEventoA == 0) {
                    nuevoEvento->setEventType("Evento A");
                    nuevoEvento->setScientist("Einstein");
                    eventoA = nuevoEvento;
                    contadorEventoA = 1;

                    cout << "Se ha producido un evento de Tipo A:" << dato << endl;
                    cout << "Este evento esta en el nodo:" << contadorNodos << endl;
                    nuevoEvento->displayEvent();
                    cout << "\n";
                } else if (datob && eventoA != nullptr && contadorEventoB == 0) {
                    nuevoEvento->setEventType("Evento B");
                    eventoB = nuevoEvento;
                    contadorEventoB = 1;

                    cout << "Se ha producido un evento de Tipo B:" << dato << endl;
                    cout << "Este evento esta en el nodo:" << contadorNodos << endl;
                    nuevoEvento->displayEvent();
                    cout << "\n";

                    contadorEventoC = 0;
                } else if (eventoA != nullptr && eventoB != nullptr && sonCoprimos(dato, eventoA->getData()) && contadorEventoC == 0) {
                    nuevoEvento->setEventType("Evento C");
                    contadorEventoC = 1;

                    cout << "Se ha producido un evento de Tipo C:" << dato << endl;
                    cout << "Este evento esta en el nodo:" << contadorNodos << endl;
                    nuevoEvento->displayEvent();
                    cout << "\n";

                    if (esPrimo(dato) && eventoA->getScientist() == "Einstein") {
                        cout << "El científico " << nuevoEvento->getScientist() << " viajó al pasado y entregó datos clave para el avance del proyecto a " << eventoA->getScientist() << endl;
                        cout << "[" << eventoA->getData() << "|" << eventoA->getScientist() << "|" << eventoA->getEventType() << "]" << " <= [" << eventoB->getData() << "|" << eventoB->getScientist() << "|" << eventoB->getEventType() << "]" << " <= [" << nuevoEvento->getData() << "|" << nuevoEvento->getScientist() << "|" << nuevoEvento->getEventType() << "]" << endl;
                        cout << "\n";
                    } else {
                        cout << "El científico " << nuevoEvento->getScientist() << " viajó al pasado, pero solo pudo observar" << endl;
                        cout << "[" << eventoA->getData() << "|" << eventoA->getScientist() << "|" << eventoA->getEventType() << "]" << " <= [" << eventoB->getData() << "|" << eventoB->getScientist() << "|" << eventoB->getEventType() << "]" << " <= [" << nuevoEvento->getData() << "|" << nuevoEvento->getScientist() << "|" << nuevoEvento->getEventType() << "]" << endl;
                        cout << "\n";
                    }
                }

                Nodo* nuevoNodo = new Nodo(nuevoEvento, nullptr, nullptr);

                if (ultimoNodo != nullptr) {
                    ultimoNodo->next = nuevoNodo;
                    nuevoNodo->prev = ultimoNodo;
                } else {
                    cabeza = nuevoNodo;
                    cola = nuevoNodo;
                }

                ultimoNodo = nuevoNodo;
            } else {
                int dato = rand() % 100 + 1;
                string cientifico = getCientifico();

                Event* nuevoEvento = new Event(dato, cientifico);

                cabeza = cola = new Nodo(nuevoEvento, nullptr, nullptr);
                contadorNodos++;
                ultimoNodo = cabeza;
            }
        }
    }

    void imprimir_Lista() {
        if (!vacio()) {
            string datos = "<=";

            Nodo* ayudante = cabeza;

            while (ayudante != nullptr) {
                datos = datos + "[" + to_string(ayudante->getEvent()->getData()) + "|" + ayudante->getEvent()->getScientist() + "|" + ayudante->getEvent()->getEventType() + "]<=";
                ayudante = ayudante->next;
            }

            cout << "Datos" << datos << endl;
        }
    }

    string getCientifico() {
        string scientist[] = { "Rosen", "Einstein" };
        int indice = rand() % 2;
        string cientificoSeleccionado = scientist[indice];
        return cientificoSeleccionado;
    }
};

int main() {
    ListaDobleEnlazada lista;

    lista.empezar();
    cout << "\n";
    lista.imprimir_Lista();

    return 0;
}
