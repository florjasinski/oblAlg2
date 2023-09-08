#include <iostream>
#include <cassert>
using namespace std;

struct _cabezalTabla;
typedef struct _cabezalTabla* TablaComida;


//La tabla de Hash
struct nodoLista{
	int pos;
	string comida;
	nodoLista *sig;


};

struct _cabezalTabla{
	nodoLista** tabla;
	int cantidad;

};

struct nodoHeap{
	int repeticiones;
	string comida;
};
 




bool existeComida(TablaComida t, string comida) {
	int pos = hashAux(d, t->cota);
	nodoLista* l = t->tabla[pos];
	while (l != NULL) {
		if (l->comida == comida) {
			return true;

		}
		l = l->sig;
	}
	return false;
}


class MaxHeap
{
private:
	nodoHeap** arr;
	int total;
	int libre;

	void intercambiar(int x, int y)
	{
		nodoHeap *aux = this->arr[x];
		this->arr[x] = this->arr[y];
		this->arr[y]= aux;
	}

	int padre(int pos){
		return pos/2;
	}

	bool hijosMenor(int pos, int posPadre){
		return this->arr[pos] < this->arr[posPadre];
	}


	
	void flotar(int pos){
		if (pos>1){
			int posPadre = padre(pos);
			if (hijosMenor(pos,posPadre)){
				intercambiar(pos,posPadre);
				flotar(posPadre);
			}

		}
	}

	int posHijoIzquierdo(int pos){
		return pos *2;
	}

	int posHijoDerecho(int pos){
		return pos *2+1;
	}

	void hundir(int pos){
		int posIzq = posHijoIzquierdo(pos);
		int posDer = posHijoDerecho(pos);
		int hijoMenor = posIzq;

		if (posIzq< this->ultimoLibre && posDer >+ this->ultimoLibre){
			if (this->arr[pos]< this->arr[posIzq]){
			intercambiar(pos,posIzq);
			hundir(posIzq);

		}


		if(posDer < this->ultimoLibre){
			if (this->arr[posIzq] > this->arr[posDer]){
			hijoMenor = posDer;
			}
		}
		if (this->arr[pos]< this->arr[hijoMenor]){
			intercambiar(pos,hijoMenor);
			hundir(hijoMenor);

		}
		}



	}

	void insertarAux(int elemento, int vieneDe){
		if (!this->estaLleno()){
			nodoHeap * nuevo = new nodoHeap;
			nuevo->dato = elemento;
			nuevo->vieneDe = vieneDe;
			this -> arr[this->ultimoLibre] = nuevo;
			this-> flotar(this->ultimoLibre);
			this->ultimoLibre++;

		}

	}

	nodoHeap * obtenerMinimoAux(){
		nodoHeap* ret = NULL;
		if (!this->esVacio()){
			ret = this->arr[1];
			this->arr[1] = this->arr[this->ultimoLibre-1];
			this->ultimoLibre--;
			hundir(1);
			
		}
		return ret;
	}

public:
	MaxHeap(int tamanio)
	{
		this->arr = new nodoHeap*[tamanio+1]();
		this->total= tamanio;
		this->libre = 1;
	}

	bool esVacio()
	{
		return this->libre ==1;
	}

	bool estaLleno()
	{
		return this->libre >= this->total+1;
	}

	void insertar(int nuevoElemento, int vieneDe){
		this->insertarAux(nuevoElemento, vieneDe);

	}
	
	nodoHeap * obtenerMinimo(){
		this->obtenerMinimoAux();


	}

};


int main(){
	int cantidadComida;
	cin >> cantidadComida;
	TablaComida tabla = new _cabezalTabla [101]();
	string comida;
	for (int i=0; i<cantidadComida; i++){
		cin >> comida;
		if (existeComida(tabla,comida)){
			//Aumentar la cantidad del arbol
		} else {
		int pos = hashAux(d, t->cota);
		nodoLista* l = new nodoLista;
		l->pos = NULL;
		l->comida = comida;
		l->sig = t->tabla[pos];
		t->tabla[pos] = l;
		//Insertar del arbol
		}
	}
}




// Hacemos el eciste si loencuentra va al hash busca la posicion y en la posicion esa comparas los trings y te da la posicion del heap. Moves el heap y la posicion del nodo del hash.
//Hacer un bool si se movio 

    
LETRA OBL: https://docs.google.com/document/d/18PWOkyKHo7aE4yqHrv65PvZ1DG0_qGlH6MldU5RvKrs/edit#heading=h.m4nhenxfjtxj
