#include <iostream>
#include <cassert>
#include <cstring>
#include <vector> // Agregar la librería vector
using namespace std;

struct nodoHeap
{
    int repeticiones;
    string comida;
};

class MaxHeap
{
private:
    nodoHeap** arr;
    int capacidad;
    int ultimoLibre;

    

    bool menor(int a, int b) {
    if (this->arr[a]->repeticiones > this->arr[b]->repeticiones) {
        return true;
    } else if (this->arr[a]->repeticiones == this->arr[b]->repeticiones) {
        // Si las repeticiones son iguales, compara las cadenas en orden alfabético
        return compararString(this->arr[a]->comida, this->arr[b]->comida);
    }
    return false;
}


    void swap(int a, int b)
    {
        nodoHeap *aux = this->arr[a];
        this->arr[a] = this->arr[b];
        this->arr[b] = aux;
        
    }

    


    void flotar(int pos) {
        if (pos > 1) {
            int posPadre = pos / 2;
            if (menor(pos, posPadre)) {
               
                swap(pos, posPadre);
                flotar(posPadre);
            }
        }
    }

    void hundir(int pos) {
        while (pos * 2 <= ultimoLibre) {
            int hijoACambiar = pos * 2;
            if (hijoACambiar + 1 <= ultimoLibre && menor(hijoACambiar + 1, hijoACambiar)) {
                hijoACambiar++;
                
            }
            if (menor(hijoACambiar, pos)) {
                swap(pos, hijoACambiar);
                pos = hijoACambiar;
                 
            } else {
                
                return;
            }
        }
    }



    void insertarAux(string comida, int repeticiones)
    {
       
        if (!this->estaLleno())
        {
            
            nodoHeap *nuevo = new nodoHeap;
            nuevo->repeticiones = repeticiones;
            nuevo->comida = comida;
            this->arr[this->ultimoLibre] = nuevo;
            flotar(this->ultimoLibre);
            this->ultimoLibre++;
             
        }
    }

    
     string obtenerYborrarTope(){
        string retorno;
        
        if (!this->esVacio())
        {
            retorno = this->arr[1]->comida;
            //delete this->arr[1]; // Liberar memoria del nodo eliminado
            this->arr[1] = this->arr[this->ultimoLibre - 1];
            this->ultimoLibre--;
            hundir(1);
            
        }
     
        return retorno;
    }

    bool compararString(string c1, string c2)
	{
		int i = 0;
		while (i != c1.length() || i != c1.length())
		{
			if (c1.at(i) == c2.at(i))
			{
				i++;
			}
			else if (c1.at(i) > c2.at(i))
			{
				return false;
			}
			else
			{
				return true;
			}
		}
	}


public:
    MaxHeap(int tamanio)
    {

        this->arr = new nodoHeap*[tamanio+1]();
        this->capacidad= tamanio;
        this->ultimoLibre = 1;
    
    }

    bool esVacio()
    {
        
        return this->ultimoLibre == 1;
    }

    bool estaLleno()
    {
        
        return this->ultimoLibre >= this->capacidad + 1;
    }

    string tope()
    {
        
        return this->obtenerYborrarTope();
    }

    void insertar(string comida, int repeticiones)
    {
    
        this->insertarAux(comida, repeticiones);
    }


    MaxHeap() {
        delete[] arr;
    }
};


	
#include <iostream>
#include <cassert>
#include <cstring>
using namespace std;

struct nodoLista {
    int repeticiones;
    string comida;
    nodoLista* sig;
};

class tablaHash {
private:
    nodoLista** tabla;
    int cantidad;

    int funcionHashAux(string comida) {
        int h = 0;
        for (int i = 0; i < comida.length(); i++) {
            h = (h * 31 + comida[i]) % 101;
        }
        return h;
    }

    void insertarComidaAux(string comida, nodoLista*& t) {
        if (t == NULL) {
            nodoLista* l = new nodoLista();
            l->repeticiones = 1;
            l->comida = comida;
            l->sig = NULL;
            t = l;
            this->cantidad++;
        } else {
            nodoLista* actual = t;
            while (actual != NULL) {
                if (actual->comida == comida) {
                    actual->repeticiones++;
                    return; // Incrementar repeticiones si el plato ya existe
                }
                actual = actual->sig;
            }
            nodoLista* nuevo = new nodoLista();
            nuevo->repeticiones = 1;
            nuevo->comida = comida;
            nuevo->sig = t;
            t = nuevo; // Agregar al principio de la lista si no se encuentra
            this->cantidad++;
        }
    }

    int obtenerRepeticionesAux() {
    for(int i=0; i<101; i++){
        nodoLista* l = tabla[i];
        if (l != NULL) {
            return l->repeticiones;
        }
    }
    return 0; // Si no se encuentra la comida, se asume que no ha sido ordenada.
    }


    string obtenerComidaAux() {
        for(int i=0; i<101; i++){
            nodoLista* l = tabla[i];
            if (l != NULL) {
                string comida = l->comida;
                nodoLista* borrar = l;
                tabla[i] = l->sig; // Actualiza el puntero de la tabla para omitir el nodo
                delete borrar; // Libera la memoria del nodo
                this->cantidad--;
                return comida;
            }
        }
        return ""; // Si no se encuentra la comida, devuelve una cadena vacía.
    }




    

public:
    tablaHash(int tamanio) {
    this->cantidad = 0;
    this->tabla = new nodoLista*[tamanio]();

    // Inicializa todos los elementos de la tabla con punteros nulos
    for (int i = 0; i < tamanio; i++) {
        tabla[i] = NULL;
    }
    }


    int funcionHash(string comida) {
        return this->funcionHashAux(comida);
    }

    void insertarComida(string comida){
        int posHash = abs(this->funcionHash(comida));
        this->insertarComidaAux(comida, tabla[posHash]);
    }

    int obtenerRepeticiones(){
        return this->obtenerRepeticionesAux();
    }

    string obtenerComida(){
        return this->obtenerComidaAux();
    }

    bool esVacia(){
        return this->cantidad==0;
    }

    ~tablaHash() {
        for (int i = 0; i < 101; i++) {
            nodoLista* current = tabla[i];
            while (current != NULL) {
                nodoLista* temp = current;
                current = current->sig;
                delete temp;
            }
        }
        delete[] tabla;
    }
};

#include <iostream>
#include <cassert>
#include <cstring>
#include "./pruebaTabla.cpp"
#include "./pruebaHeap.cpp"

using namespace std;

int main() {
    int cantidadComida;
    cin >> cantidadComida;
    tablaHash* t = new tablaHash(101);
    string comida;

    for (int i = 0; i < cantidadComida; i++) {
        cin >> comida;
        t->insertarComida(comida);
    }

    MaxHeap *heap = new MaxHeap(cantidadComida);
    while (!(t->esVacia())){
        int repeticiones = t->obtenerRepeticiones(); // Obtener las repeticiones desde la tabla hash
        string comidaTabla = t->obtenerComida();
        heap->insertar(comidaTabla, repeticiones); // Insertar la comida con sus repeticiones
    }

    while (!heap->esVacio()) {

        cout << heap->tope() << endl;
    }

    delete t; 
    delete heap; 

    return 0;
}
