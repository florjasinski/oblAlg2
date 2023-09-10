#include <iostream>
#include <cassert>
#include <cstring>
using namespace std;

struct _cabezalTabla;
typedef struct _cabezalTabla* TablaComida;


//La tabla de Hash
struct nodoLista{
	int pos;
	char* comida;
	nodoLista *sig;


};

struct _cabezalTabla{
	nodoLista** tabla;
	int cantidad;

};

struct nodoHeap{
	int repeticiones;
	char* comida;
};
 

char* copiaChar(char* rango) {
	assert(rango != NULL);
	char pos = rango[0];
	int largo = 0;
	while (pos != '\0') {
		largo++;
		pos = rango[largo];
	}
	char* nuevo = new char[largo + 1];
	nuevo[largo] = '\0';
	for (int i = 0; i < largo; i++) {
		nuevo[i] = rango[i];
	}
	return nuevo;
}
int hashAux(char* palabra){
	int h =0;
	for(int i= 0; palabra[i]!='\0'; i++){
		h=h +palabra[i]*(i+1);
	}
	return h;
}
	

bool existeComida(TablaComida t, char* comida) {
	int pos = hashAux(comida);
	nodoLista* l = t->tabla[pos];
	while (l != NULL) {
		if (strcmp(l->comida, comida) == 0) {
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

	bool menor(int a, int b) {
        return this->arr[a] < this->arr[b];
	}


	void swap(int a, int b) {
        nodoHeap *aux = this->arr[a];
		this->arr[a] = this->arr[b];
		this->arr[b]= aux;
    }

	void flotar(int pos) {
        if (pos > 1){
			int posPadre = pos/2;
			if (menor(this->arr[pos]->repeticiones,this->arr[posPadre]->repeticiones)){
				swap(pos,posPadre);
				flotar(posPadre);
			}
    	}
	}


	 void hundir(int pos) {
        while (pos * 2 <= this->total) {
            int hijoACambiar = pos * 2;
            if (hijoACambiar + 1 <= this->total && menor(this->arr[hijoACambiar + 1]->repeticiones, this->arr[hijoACambiar]->repeticiones)) {
                hijoACambiar++;
            }
            if (menor(this->arr[hijoACambiar]->repeticiones, this->arr[pos]->repeticiones)) {
                swap(pos, hijoACambiar);
                pos = hijoACambiar;
            } else
                return;
        }
    }

	void insertarAux(char* comida){
		if (!this->estaLleno()){
			nodoHeap * nuevo = new nodoHeap;
			nuevo->repeticiones = 1;
			nuevo->comida = comida;
			this -> arr[this->libre] = nuevo;
			//this-> flotar(this->libre);
			this->libre++;

		}

	}

	char* obtenerYborrarTope(){
		char* retorno = NULL;
		if (!this->esVacio()){
			retorno = this->arr[1]->comida;
			this->arr[1] = this->arr[this->libre-1];
			this->libre--;
			hundir(1);
			
		}
		return retorno;
	}


	int devPosYAumentarRepAux(int pos){
		int ret = 0;
		char* copia = copiaChar(this->arr[pos]->comida);
		this->arr[pos]->repeticiones++;
		this-> flotar(this->arr[pos]->repeticiones);
		for(int i= 1; i<101; i++){
			if (this->arr[i]->comida == copia){
			ret = i;
			}
		}
		return ret;
	}


	int devolverLibreAux(){
		return this->libre;
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

	
	
	char* tope(){
		return this->obtenerYborrarTope();


	}


	void insertar(char* comida){
		this->insertarAux(comida);
	}


	int devPosYAumentarRep(int pos){
		return this->devPosYAumentarRepAux(pos);
	}

	int devolverLibre(){
		return this->devolverLibreAux();
	}

};


int main(){
	int cantidadComida;
	cin >> cantidadComida;
	TablaComida t = new _cabezalTabla [101]();
	char* comida;
	int posHash;
	MaxHeap* heap = new MaxHeap (cantidadComida);
	for (int i=0; i<cantidadComida; i++){
		cin >> comida;
		if (existeComida(t,comida)){
			int pos= hashAux(comida);
			nodoLista* l = t->tabla[pos];
			while(l!=NULL&& l->comida!=comida){
				l=l->sig;
			}
			posHash = heap->devPosYAumentarRep(l->pos);
			l->pos = posHash;
			
		} else {
		int pos = hashAux(comida);
		nodoLista* l = new nodoLista;
		l->pos = heap->devolverLibre() ;
		l->comida = comida;
		l->sig = t->tabla[pos];
		t->tabla[pos] = l;
		heap->insertar(comida);
		}
	}
	while(!heap->esVacio()){
		cout << heap->tope() << endl;
	}
}

    
LETRA OBL: https://docs.google.com/document/d/18PWOkyKHo7aE4yqHrv65PvZ1DG0_qGlH6MldU5RvKrs/edit#heading=h.m4nhenxfjtxj
