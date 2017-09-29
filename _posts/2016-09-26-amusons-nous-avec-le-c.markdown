---
layout: default
title: "Amusons-nous avec le C"
date: 2016-09-26 20:20:03
permalink: /:title/
---
Le C est un langage que je n'ai pas pratiqué depuis bien longtemps. Il faut dire, hormis la programmation système et quelques niches, on ne le voit pas trop ailleurs. En tout cas au travail, je n'ai jamais eu à en faire. Et d'un point de vue personnel, je suis plus sur d'autres langages comme Python ou Haskell. Mais il n'empêche que j'ai stocké, au gré de mes pérégrinations, **quelques liens intéressants** sur ce qu'on peut faire d'amusant en poussant ce langage hors des sentiers battus.

<!--excerpt-->

## Une classe string

Toute personne ayant fait du C le dira, la gestion des chaînes de caractères est très ... sommaire. Allouer des tableaux de char soi-même, c'est sympa 5 min, mais bon, si l'on avait une vraie encapsulation, comme les classes string disponibles dans quasiment tous les langages venus après C, ça serait mieux. Ça tombe bien, [Maëlan](https://zestedesavoir.com/membres/voir/Ma%C3%ABlan/) l'a fait, à base de listes chaînées pour stocker des sous-chaînes et faciliter les opérations de concaténation, etc. Le code a été posté de base [sur le Site du Zéro](https://openclassrooms.com/forum/sujet/une-implementation-de-string-en-c-75483#message-6801924), mais par soucis de postérité, on va le remettre ici.

{% highlight c %}
/**
***  String.h
***
***    module:   String  −  header file
***    function: provides the `String` object type (abstract data type) and
***              functions for using it; the `String` object type is designed to
***              manage dynamic and/or large characters strings.
***              This header is the interface file of the module, it provides
***              the incomplete type declaration of `String` (because `String`
***              is an ADT) and the prototype of the usable functions.
***    author:   Maëlan (aka Maëlan44)
***              (see http://www.siteduzero.com/membres-294-232877.html )
***    website:  http://www.siteduzero.com/forum-83-683766-p1-une-implementation-de-string-en-c.html
***
***    last modified: 09/07/2011, 15:15
***
**/

#ifndef INCLUDED_STRING_2011_08_25_17_30_MM
#define INCLUDED_STRING_2011_08_25_17_30_MM

#include <stddef.h>   // for size_t
#include <stdio.h>    // for FILE

/*** TYPES ***/

/* the String object, to manage large and/or dynamic strings;
   it takes the form of a linked list of datas including an allocated block
   and informations concerning this block and the linked list */
typedef  struct String  String;

/*** FUNCTIONS ***/

/* Creates a String object filled with the content of the string passed; if NULL
   is passed,  lets it null (ie. empty and with no capacity) */
String* String_new(const char*);

/* Creates an empty String object with at least the capacity specified */
String* String_newEmpty(size_t);

/* Creates a new String object that is a copy of the passed one */
String* String_copy(const String*);

/* Deletes completely the String object */
void String_del(String*);

/* Returns the size of the String */
/*extern inline*/  size_t String_size(const String*);
/* idem */
#define  String_length  String_size

/* Returns the capacity of the String */
/*extern inline*/  size_t String_capacity(const String*);

/* boolean: Is the String empty (ie. the size is zero)? */
/*extern inline*/  int String_isEmpty(const String*);

/* boolean: Is the String null (ie. the size and capacity are zero)? */
/*extern inline*/  int String_isNull(const String*);

/* Empties the String, setting its size to zero without altering its capacity */
void String_empty(String*);

/* Empties the String, setting its size and capacity to zero */
void String_clear(String*);

/* Returns the character of index specified in the String; returns '\\0' if the
   index is out of range */
char String_char(/*const*/ String*, size_t);

/* Sets the character of index specified in the String to the value specified;
   if it is '\\0', then this index will become the end of the String; if the
   index is out of range, the linked String is not modified */
void String_setChar(String*, size_t, char);

/* compare the two Strings the same way as strcmp does */
int String_cmp(const String*, const String*);

/* compare the String object with the ”regular” characters string, the same way
   as strcmp does */
int String_cmpCStr(const String*, const char*);

/* returns the index of the 1st occurrence of the character in the String, or a
   negative number if the the character is no found */
ptrdiff_t String_searchChar(const String*, char c);

/* returns the index of the last occurrence of the character in the String, or a
   negative number if the the character is no found */
ptrdiff_t String_searchLastChar(const String*, char c);

/* Concatenates a copy of the 2nd String object to the first; returns the final
   String, or NULL if there was an error */
String* String_concat(String*, const String*);

/* Write the content of the String on the stream */
void String_fput(const String*, FILE*);

/* idem, the stream is `stdout` */
/*extern inline*/  void String_put(const String*);

/* Returns a buffer containing the content of the String in the form of a plain
   characters string; this buffer shall not be modified by the user, and it may
   be modified by the String functions to stay up-to-date or be freed if it is
   no longer the case */
const char* String_cStr(String*);

/* Returns the buffer containing the "cstr" of the String if it exists and is
   up-to-date, NULL otherwise */
/*extern inline*/  const char* String_getCStr(const String*);

#endif

/**
***  String.c
***
***    module:   String  −  source file
***    function: provides the `String` object type (abstract data type) and
***              functions for using it; the `String` object type is designed to
***              manage dynamic and/or large characters strings.
***              This file is the main source file of the module, it contains
***              the full declaration of the type `String`, as well as others
***              types and additionnal function declarations (static), both used
***              internally by the implementation. Of course it contains (in
***              fact, includes) also the definitions of all the functions.
***    author:   Maëlan (aka Maëlan44)
***              (see http://www.siteduzero.com/membres-294-232877.html )
***    website:  http://www.siteduzero.com/forum-83-683766-p1-une-implementation-de-string-en-c.html
***
***    last modified: 09/06/2011, 18:40
***
**/

#include "String.h"
#include <stdlib.h>
#include <string.h>
#include <stdio.h>
#include "error.h"

/*** CONSTANTS ***/

/* see below */
#define  STRING_LOG2BLKSZ      8

/* maximal lenght of the buffer of a String block */
#define  STRING_TOTALMAXBLKSZ  (1<<STRING_LOG2BLKSZ)

/* maximal lenght of the buffer of a String block, without the terminating '\\0' */
#define  STRING_MAXBLKSZ       (STRING_TOTALMAXBLKSZ-1)

/* lenght of the static buffer of a String block; it is designed for a struct of
   type `StringBlock` to fit in 32 bytes */
//#define  STRING_TOTALBUFSZ     ( (32-sizeof(*char)-2*STRING_LOG2BLKSZ/8)       \\
								 / sizeof(char) )
#define STRING_TOTALBUFSZ 19

/* lenght of the static buffer of a String block, without the terminating '\\0' */
#define  STRING_BUFSZ          (STRING_TOTALBUFSZ-1)

/*** TYPES ***/

/* NOTE:
 * Here, "block capacity" will mean the allocated length for the block minus 1,
 * the latter being reserved for the terminating '\\0'. The "String capacity" or
 * "total capacity" will mean the sum of the capacities of all the blocks
 * composing the String.
 * Similarly, "block size" will mean the used length without the '\\0'. The
 * "String size" of "total size" is the sum of the sizes of all the blocks.
 */

/* a String block, link of a linked list;
   contains a buffer and informations for using it */
typedef struct StringBlk {
	/* static buffer: optimisation for short strings or string blocks
	   [OPT:buf] */
	char buf[STRING_TOTALBUFSZ];
	/* buffer; shall point to the member `buf` if it is used, else to the
	   allocated memory
	   /!\\  Shall never be NULL! */
	char* p;
	/* block size (shall be lesser than or equal to the member `capac`) */
	unsigned int size   :STRING_LOG2BLKSZ,
	/* block capacity (shall be at least STRING_BUFSZ, since it is the size
	   of the static buffer; can not be greater than STRING_MAXBLKSZ) */
				 capac  :STRING_LOG2BLKSZ;
	/* block following this one in the linked list */
	struct StringBlk* next;
} StringBlk;

/* the String object itself;
   it takes the form of a linked list of blocks, each including a buffer and
   informations concerning it */
struct String {
	/* total length */
	size_t  size,
	/* total capacity */
			capac;
	/* first block of the string */
	StringBlk *first,
	/* last block: optimisation for operations on the end of the string,
	   such as concatenating or adding a character [OPT:last] */
			  *last;
	/* optimisation: a cursor pointing to the last character read [OPT:cur] */
	struct {   size_t pos;   /* index of the 1st char. of the block pointed */
			   StringBlk* blk;
		   } cur;
	/* buffer containing the full character string obtained via
	   `String_cStr` (and returned as constant); shall be NULL if this
	   function had not be called yet, or if the content is outdated (in
	   this case, it shall be freed if needed).
	   For reasons of optimisation, this buffer can be that of the first
	   block of the String, in which case it shall not be freed.
	   If the buffer exists, it can be used to perform optimisations on
	   operations involving reading content [OPT:cstr]. */
	char* cStr;
};

/*** AUXILIARY FUNCTIONS ***/

/* returns the power of 2 greater than or equal to `n` */
static  unsigned int pow2sup(unsigned int n) {

	if(!n)   return 0;
	unsigned int r= 1;
	while(n>r)   r<<=1;
	return r;

		/**   // solution found here < http://forum.hardware.fr/hfr/Programmation/C-2/quizz-calculer-superieure-sujet_52944_1.htm#t757526 >
		if(!n || !(n&(n-1))   return n;
		__asm__ (
			"bsr   eax,   n"
			"mov   n,   eax"
		);
		return 1<<n;
		**/
}

/*** MAIN FUNCTIONS ***/

/** functions for object `StringBlock` **/

/* Creates a new String block filled with the content of the string passed, or
   at most its STRING_MAXBLKSZ first characters; the second argument must be the
   size of the string passed */
static  StringBlk* StringBlk_new(const char*, size_t);

/* Creates a new empty String block with at least the specified capacity */
static  StringBlk* StringBlk_newEmpty(unsigned int);

/* Creates a copy of the String block (ie. a new String block filled with the
   content of the one passed) */
static inline  StringBlk* StringBlk_copy(const StringBlk*);

/* Deletes the String block and all the following String blocks of the linked
   list to which it belongs */
static  void StringBlk_del(StringBlk*);

/* boolean: Does the String block uses its static buffer? */
static inline  int StringBlk_useStaticBuf(const StringBlk* this);

#include "String.c.1"

/** functions for object `String` **/

/* boolean: Has the String an existing "cstr"? */
static inline  int String_hasCStr(const String*);

/* boolean: Has the String an existing "cstr", the buffer of which is also that
   of the 1st block? */
static inline  int String_hasSharedCStr(const String*);

/* boolean: Has the String an existing "cstr", the buffer of which is allocated
   separately? */
static inline  int String_hasSeparateCStr(const String*);

/* Delete the "cstr" of the String */
static inline  void String_delCStr(String*);

#include "String.c.2"

/**
***  String.c.1
***
***    module:   String  −  source file (part 1)
***    function: provides the `String` object type (abstract data type) and
***              functions for using it; the `String` object type is designed to
***              manage dynamic and/or large characters strings.
***              This file contains the definitions of functions working on the
***              `StringBlock` object type.
***    author:   Maëlan (aka Maëlan44)
***              (see http://www.siteduzero.com/membres-294-232877.html )
***    website:  http://www.siteduzero.com/forum-83-683766-p1-une-implementation-de-string-en-c.html
***
***    last modified: 09/07/2011, 20:45
***
**/

static  StringBlk* StringBlk_newEmpty(unsigned int capac) {
	StringBlk* r =  malloc(sizeof(StringBlk));
	if(r==NULL) {
		errorMessage("malloc", "when allocating a String block");
		return NULL;
	}

	if(capac<=STRING_BUFSZ) {
		r->p =      r->buf;
		r->capac =  STRING_BUFSZ;
	} else {
		r->capac =  (capac>STRING_MAXBLKSZ)? STRING_MAXBLKSZ : capac;
		//r->capac =  log2sup(r->blk.capac) - 1;
		r->p =  malloc((r->capac+1)*sizeof(char));
		if(r->p == NULL) {
			errorMessage("malloc", "when allocating a dynamic"
			 " buffer for a String block");
			free(r);
			return NULL;
		}
	}

	*r->p =    '\\0';
	r->size =  0;
	//r->next =      NULL;

	return r;
}

static  StringBlk* StringBlk_new(const char* s, size_t size) {
	StringBlk* r;

	//if(s==NULL)    size =  0;
	r =  StringBlk_newEmpty(size);
	if(r==NULL)    return NULL;

	r->size =  (size > r->capac)? r->capac : size;
	/*if(s!=NULL)    */strncpy(r->p, s, r->size);
	r->p[r->size] =  '\\0';

	return r;
}

static inline  StringBlk* StringBlk_copy(const StringBlk* this)
	{ //if(this==NULL)    return NULL;
	return StringBlk_new(this->p, this->size); }

static  void StringBlk_del(StringBlk* this) {
	StringBlk* blk;
	while(this!=NULL) {
		blk =   this;
		this =  this->next;
		if(!StringBlk_useStaticBuf(blk))
			free(blk->p);
		free(blk);
	}
}

static inline  int StringBlk_useStaticBuf(const StringBlk* this)
	{ return this->p == this->buf; }

/**
***  String.c.2
***
***    module:   String  −  source file (part 2)
***    function: provides the `String` object type (abstract data type) and
***              functions for using it; the `String` object type is designed to
***              manage dynamic and/or large characters strings.
***              This file contains the definitions of functions working on the
***              `String` object type.
***    author:   Maëlan (aka Maëlan44)
***              (see http://www.siteduzero.com/membres-294-232877.html )
***    website:  http://www.siteduzero.com/forum-83-683766-p1-une-implementation-de-string-en-c.html
***
***    last modified: 09/07/2011, 18:50
***
**/

/**  CREATION, DELETION
 **    newEmpty, new, copy, del
 **/

String* String_newEmpty(size_t capac) {
	String* r;
	StringBlk** p;

	r =  malloc(sizeof(String));
	if(r==NULL) {
		errorMessage("malloc", "when allocating a String");
		return NULL;
	}

	r->capac =  0;
	p =  &r->first;
	do {    /* at least one block (so STRING_BUFSZ) is created, even if the
			   capacity required is 0 */
		*p =  StringBlk_newEmpty(capac-r->capac);
		if(*p==NULL) {
			errorMessage("StringBlk_newEmpty", "when creating a"
			 " String block");
			String_del(r);
			return NULL;
		}
		r->capac +=  (*p)->capac;
		if(capac > r->capac)    p =  &(*p)->next;
	} while(capac > r->capac);

	r->last =     *p;
	(*p)->next =  NULL;

	r->size =     0;
	r->cur.pos =  0;
	r->cur.blk =  r->first;
	r->cStr =     r->first->p;

	return r;
}

String* String_new(const char* s) {
	String* r;

	r =  malloc(sizeof(String));
	if(r==NULL) {
		errorMessage("malloc", "when allocating a String");
		return NULL;
	}

	r->size =   0;
	r->capac =  0;
	if(s==NULL) {
		r->first =  NULL;
		r->last =   NULL;
	} else {
		size_t size =  strlen(s);
		StringBlk** p =  &r->first;
		do {
			*p =  StringBlk_new(s+r->size, size-r->size);
			if(*p==NULL) {
				errorMessage("StringBlk_new", "when creating a"
				 " String block");
				String_del(r);
				return NULL;
			}
			r->capac +=  (*p)->capac;
			r->size +=   (*p)->size;
			if(size != r->size)    p =  &(*p)->next;
		} while(size != r->size);
		r->last =     *p;
		(*p)->next =  NULL;
	}

	r->cur.pos =  0;
	r->cur.blk =  r->first;
	r->cStr =     NULL;

	return r;
}

String* String_copy(const String* this) {
	if(this==NULL)    return NULL;

	if(String_hasCStr(this))
		return String_new(this->cStr);

	String* r;

	r =  malloc(sizeof(String));
	if(r==NULL) {
		errorMessage("malloc", "when allocating a String");
		return NULL;
	}

	StringBlk *blk =  this->first,
			  **p =   &r->first;
	r->size =   0;
	r->capac =  0;
	if(blk==NULL)    r->first =  NULL;
	else while(blk!=NULL) {
		*p =  StringBlk_copy(blk);
		if(*p==NULL) {
			errorMessage("StringBlk_copy", "when creating a copy of"
			 " a String block");
			 String_del(r);
			 return NULL;
		}
		r->capac +=  (*p)->capac;
		r->size +=   (*p)->size;
		blk =  blk->next;
		if(blk!=NULL)    p =  &(*p)->next;
	}
	r->last =     *p;
	(*p)->next =  NULL;
	r->cur.pos =  0;
	r->cur.blk =  r->first;
	r->cStr =     NULL;

	return r;
}

void String_del(String* this) {
	if(this==NULL)    return;
	if(String_hasSeparateCStr(this))
		free(this->cStr);
	if(!String_isNull(this))
		StringBlk_del(this->first);
	free(this);
}

/**  SIZE, CAPACITY
 **    size, capacity, isEmpty, isNull, empty, clear
 **/

inline  size_t String_size(const String* this)
	{ return (this!=NULL)? this->size : 0; }

inline  size_t String_capacity(const String* this)
	{ return (this!=NULL)? this->capac : 0; }

inline  int String_isEmpty(const String* this)
	{ return (this!=NULL)? !this->size : 1; }

inline  int String_isNull(const String* this)
	{ return (this!=NULL)? !this->capac : 1; }

void String_empty(String* this) {
	if(this==NULL)    return;
	StringBlk* p =  this->first;
	while(p!=NULL) {
		*p->p =    '\\0';
		p->size =  0;
		p =        p->next;
	}
	this->size =  0;
}

void String_clear(String* this) {
	if(this==NULL)    return;
	if(String_hasCStr(this)) {
		if(String_hasSeparateCStr(this))
			free(this->cStr);
		this->cStr =  NULL;
	}
	if(!String_isNull(this)) {
		StringBlk_del(this->first);
		this->first =  NULL;
		this->last =   NULL;
	}
	this->capac =    0;
	this->size =     0;
	this->cur.pos =  0;
	this->cur.blk =  NULL;
}

/**  CSTR
 **    getCStr, delCStr, hasCStr, hasSharedCStr, hasSeparateCStr
 **/

inline  const char* String_getCStr(const String* this)
	{ return this->cStr; }

void String_delCStr(String* this) {
	free(this->cStr);
	this->cStr =  NULL;
}

static inline  int String_hasCStr(const String* this)
	{ return this->cStr != NULL; }

static inline  int String_hasSharedCStr(const String* this)
	{ return this->capac  &&  this->cStr == this->first->p; }

static inline  int String_hasSeparateCStr(const String* this)
	{ return this->cStr != NULL  &&  this->cStr != this->first->p; }

/**  BASIC READ / WRITE
 **    char, setChar
 **/

char String_char(/*const*/ String* this, size_t i) {
	if(this==NULL || i>=this->size)    return '\\0';

	if(String_hasCStr(this))    return this->cStr[i];

	if(i < this->cur.pos) {
		this->cur.pos =  0;
		this->cur.blk =  this->first;
	}
	while(i >= this->cur.pos + this->cur.blk->size) {
		this->cur.pos +=  this->cur.blk->size;
		this->cur.blk =   this->cur.blk->next;
	}
	return this->cur.blk->p[i - this->cur.pos];
}

void String_setChar(String* this, size_t i, char c) {
	if(this==NULL || i>=this->size)    return;

	if(i < this->cur.pos) {
		this->cur.pos =  0;
		this->cur.blk =  this->first;
	}
	while(i >= this->cur.pos + this->cur.blk->size) {
		this->cur.pos +=  this->cur.blk->size;
		this->cur.blk =   this->cur.blk->next;
	}
	this->cur.blk->p[i-this->cur.pos] =  c;

	if(c=='\\0') {
		this->size =  i;
		this->cur.blk->size =  i - this->cur.pos;
		this->last =  this->cur.blk;
		if(this->last->next!=NULL) {
			StringBlk_del(this->last->next);
			this->last->next =  NULL;
		}
	}

	if(String_hasSeparateCStr(this)) {
		this->cStr[i] =  c;
		if(c=='\\0') {
			// cf here: < http://www.siteduzero.com/forum-83-704272-p1-causes-d-echec-de-realloc.html >
			char* tmp =  realloc(this->cStr, (i+1)*sizeof(char));
			if(tmp==NULL)
				String_delCStr(this);
			else
				this->cStr =  tmp;
		}
	}
}

/**  UTILITIES
 **    cmp, cmpCStr, searchChar, searchLastChar
 **/

int String_cmp(const String* s1, const String* s2) {
	if(String_isEmpty(s1))    return !String_isEmpty(s2);
	if(String_isEmpty(s2))    return -1;

	if(String_hasCStr(s1) && String_hasCStr(s2))
		return strcmp(s1->cStr, s2->cStr);

	StringBlk *b1 =  s1->first,
			  *b2 =  s2->first;
	size_t i1 =  0,
		   i2 =  0;

	while(b1->p[i1] == b2->p[i2]) {
		if(b1->p[i1]=='\\0')    return 0;
		++i1;    ++i2;
		while(i1 >= b1->size) {    /* while and not if, since there can
									 be empty blocks */
			i1 =  0;
			b1 =  b1->next;
			if(b1==NULL)    return s1->size < s2->size;
		}
		while(i2 >= b2->size) {
			i2 =  0;
			b2 =  b2->next;
			if(b2==NULL)    return -(s1->size > s2->size);
		}
	}
	return  (b1->p[i1] > b2->p[i2])?  -1 : 1;
}

int String_cmpCStr(const String* this, const char* s) {
	if(String_isEmpty(this))    return s!=NULL && *s!='\\0';
	if(s==NULL || *s=='\\0')     return -1;

	if(String_hasCStr(this))
		return strcmp(this->cStr, s);

	StringBlk *blk =  this->first;
	size_t i =  0,
		   j =  0;

	while(blk->p[i] == s[j]) {
		if(s[j]=='\\0')    return 0;
		++i;    ++j;
		while(i >= blk->size) {    /* while and not if, since there can
									 be empty blocks */
			i =    0;
			blk =  blk->next;
			if(blk==NULL)    return s[j]!='\\0';
		}
	}
	return  (blk->p[i] > s[j])?  -1 : 1;
}

ptrdiff_t String_searchChar(const String* this, char c) {
	if(String_isEmpty(this))    return (c=='\\0')? 0 : -1;
	if(c=='\\0')                 return this->size;

	if(String_hasCStr(this))    return strchr(this->cStr, c) - this->cStr;

	StringBlk* blk =  this->first;
	size_t pos =      0;
	char* r;
	while(blk!=NULL) {
		r =  strchr(blk->p, c);
		if(r!=NULL)    return r - blk->p + pos;
		pos +=  blk->size;
		blk =   blk->next;
	}
	return -1;
}

ptrdiff_t String_searchLastChar(const String* this, char c) {
	if(String_isEmpty(this))    return (c=='\\0')? 0 : -1;
	if(c=='\\0')                 return this->size;

	if(String_hasCStr(this))    return strrchr(this->cStr, c) - this->cStr;

	StringBlk* blk =   this->first;
	size_t pos =       0;
	char* r;
	ptrdiff_t found =  -1;
	while(blk!=NULL) {
		r =  strrchr(blk->p, c);
		if(r!=NULL)    return found =  r - blk->p + pos;
		pos +=  blk->size;
		blk =   blk->next;
	}
	return found;
}

/**  INSERTION
 **    concat
 **/

String* String_concat(String* this, const String* s2) {
	if(this==NULL)    return NULL;
	if(s2==NULL)      return this;

	String* cpy =  String_copy(s2);
	if(cpy==NULL) {
		errorMessage("String_copy", "when copying a String to add it to"
		 " the end of another one");
		return NULL;
	}

	if(String_isNull(this))    this->first =       cpy->first;
	else                       this->last->next =  cpy->first;
	this->last =    cpy->last;
	this->capac +=  cpy->capac;
	this->size +=   cpy->size;
	/*  /!\\  UPDATE CSTR DATAS */

	free(cpy);
	return this;
}

/**  IN / OUT
 **    fput, put
 **/

void String_fput(const String* this, FILE* stream) {
	if(this==NULL || stream==NULL)    return;
	StringBlk* p =  this->first;
	while(p!=NULL) {
		fputs(p->p, stream);
		p =  p->next;
	}
}

inline  void String_put(const String* this)
	{ String_fput(this, stdout); }
{% endhighlight %}

## Une implémentation de vector

De même que pour string, on ne dispose de base en C d'**aucune abstraction pour un conteneur dynamique** comme on en a en C++, Python, etc. Et bon, faire 40 malloc / free par fonction, c'est sympa 5 min. Mais c'était sans compter sur les expériences que [Qnope](https://openclassrooms.com/membres/qnope) a fait subir au préprocesseur (disponible [ici](https://openclassrooms.com/forum/sujet/vector-stack-queue-en-preprocesseur-24023?page=1)) et dont nous remettrons le code ici dans un soucis de disponibilité. Notons au passage que le préprocesseur est souvent utilisé pour [implémenter une pseudo-généricité en C](https://richarddegenne.wordpress.com/2015/12/27/la-programmation-generique-en-c/).

Commençons par la gestion des erreurs.

{% highlight c %}
#ifndef ERROR_H_INCLUDED
#define ERROR_H_INCLUDED

typedef enum{
	/** Succès **/
	QGM_SUCCESS,            /** Rien à signalé **/

	/** Mauvaise allocations **/
	QGM_BADALLOC,           /** Allocation de mémoire échouée **/

	/** Fuites de mémoires **/
	QGM_MEMORYLEAKSALLOC,   /** Fuites de mémoires  (Allocation) **/

	/** L'élément n'existe pas **/
	QGM_NOELEMENTALLOC,     /** Pointeur à détruire n'existe pas **/
	QGM_NOELEMENTSTACK,     /** Aucun élément à dépiler **/
	QGM_NOELEMENTQUEUE,     /** Aucun élement à défiler **/
	QGM_NOELEMENTVECTOR,    /** Aucun élément à supprimer **/
}QGM_Error_t;

/** Variable recevant l'erreur **/
extern QGM_Error_t QGM_Error;

/** Synopsis    : Indique si il y a ou non une erreur
	Retour      : 1 : erreur. 0 : Pas d'erreur **/
int QGM_IsError(void);

/** Synopsis    : Permet de récupèrer l'erreur
	Retour      : Chaîne de caractère contenant l'erreur **/
char const *QGM_GetError(void);

#endif

#include <stdio.h>

#include "error.h"

/** Variable recevant l'erreur **/
QGM_Error_t QGM_Error = QGM_SUCCESS;

/** Tableau des erreurs en fonction de QGM_Error **/
char const *QGM_String_Error[] = {
/** Succès **/
"",

/** Mauvaise allocations **/
"Not space\\n",

/** Fuites de mémoires **/
"Memory Leaks Are Detected (Allocation)\\n",

/** L'élément n'existe pas **/
"This pointer can't be free (Allocation)\\n",
"Nothing element to pop (Stack)\\n",
"Nothing element to pop (Queue)\\n",
"Nothing element to delete (Vector)\\n",
};

int QGM_IsError(void){
	return (QGM_Error == QGM_SUCCESS) ? 0 : 1;
}

/** Fonction permettant de récupérer l'erreur **/
char const *QGM_GetError(void)
{
	char const *t = QGM_String_Error[QGM_Error];
	QGM_Error = QGM_SUCCESS;
	return t;
}
{% endhighlight %}

Maintenant, le code qui nous intéresse vraiment (attention, détournement de préprocesseur en approche).

{% highlight c %}
#ifndef DATA_H_INCLUDED
#define DATA_H_INCLUDED

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "error.h"

/** Définit la Pile
		-Un tableau dynamique
		-Le nombre d'élément
		-Le nombre d'élément allouer
		-Le nombre d'élément maximale :~(size_t)0;
		-Un pointeur inutile pour la réallocation **/
#define QGM_STACK(TYPE)\\
struct{\\
	TYPE *data;\\
	size_t nElements;\\
	size_t sizeAlloc;\\
	size_t sizeMaxAlloc;\\
	TYPE *unusedPointer;\\
}*

/** Définit la File
		-Un tableau dynamique
		-Le nombre d'élément
		-Le nombre d'élément allouer
		-Le nombre d'élément maximale : ~(size_t)0;
		-L'index de fin de file
		-L'index de début de file
		-Un pointeur inutile pour la réallocation
		-Une valeure inutile pour accélérer la file si push = pop d'élément **/
#define QGM_QUEUE(TYPE)\\
struct{\\
	TYPE *data;\\
	size_t nElements;\\
	size_t sizeAlloc;\\
	size_t sizeMaxAlloc;\\
	unsigned push;\\
	unsigned pop;\\
	TYPE *unusedPointer;\\
	TYPE  unusedData;\\
}*

/** Définit le Vector
	-Un tableau dynamique
	-Le nombre d'élément
	-Le nombre d'élément allouer
	-Le nombre d'élément maximale : ~(size_t)0;
	-Un pointeur inutile pour la réallocation
	-Une valeure inutile pour certains retraits **/
#define QGM_VECTOR(TYPE) \\
struct{\\
	TYPE *data;\\
	size_t nElements;\\
	size_t sizeAlloc;\\
	size_t sizeMaxAlloc;\\
	TYPE *unusedPointer;\\
	TYPE unusedData;\\
}*

/** Initialise la structure
		-Créer la structure
		-Création d'un élément non initialisé
		-Paramètre les attributs :
			-0 élément
			-1 élément pré-alloué
			-maxAlloc = maxSize_t / taille d'une donnée
		Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_INIT(self)\\
((((self) = (malloc)(sizeof *(self))) != NULL) ?\\
		((((self)->data = (malloc)(sizeof *(self)->data)) != NULL) ?\\
			((self)->nElements = 0,\\
			 (self)->sizeAlloc = 1,\\
			 (self)->sizeMaxAlloc = ~(size_t)0 / sizeof(*(self)->data),\\
			  QGM_SUCCESS)\\
		:\\
			(free((self)),\\
			 QGM_Error = QGM_BADALLOC,\\
			 QGM_BADALLOC))\\
:\\
	(QGM_Error = QGM_BADALLOC, QGM_BADALLOC))\\

/** Réalloue les données des Stacks, Vector, Queue
		-On vérifie que la taille ne dépassera pas un size_t.
		-On realloue une taille plus grande.
		-On réinitialise les paramètres, et on renvoie QGM_SUCCESS.

		Si pas d'erreur, retorune QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_RESIZE(self)\\
(((self)->sizeAlloc * 3 <= (self)->sizeMaxAlloc) ?\\
		((((self)->unusedPointer = (realloc)((self)->data,\\
											 (self)->sizeAlloc * 3 *\\
											  sizeof *(self)->data))\\
									!= NULL) ? \\
			 ((self)->data = (self)->unusedPointer,\\
			  (self)->sizeAlloc *= 3,\\
			  QGM_SUCCESS)\\
		:\\
			(QGM_Error = QGM_BADALLOC,\\
			 QGM_BADALLOC))\\
	:\\
		(QGM_Error = QGM_BADALLOC,\\
		 QGM_BADALLOC))

/** QGM_SIZE : Renvoie le nombre d'élément **/
#define QGM_SIZE(self) (self)->nElements

/** EMPTY : Renvoie 1 si vide, sinon 0 **/
#define QGM_EMPTY(self) !(self)->nElements

/** QGM_QUIT : Détruit la structure **/
#define QGM_QUIT(self) (free((self)->data), free(self))

/*****************************************************************************/
/*********************************** STACK ***********************************/
/*****************************************************************************/

/** Initialise la Pile
		Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_STACK_INIT(self) QGM_INIT(self)

/** Push : Ajoute un élément sur la pile.
		-Véfifie si il y a de la place, sinon realloue
		-Ajoute _data sur la pile

		Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_STACK_PUSH(self, _data)\\
(((self)->nElements == (self)->sizeAlloc) ?\\
	((QGM_RESIZE(self) == QGM_SUCCESS) ?\\
		((self)->data[(self)->nElements++] = (_data), QGM_SUCCESS)\\
	:\\
		(QGM_Error = QGM_BADALLOC, QGM_BADALLOC))\\
:\\
	((self)->data[(self)->nElements++] = (_data), QGM_SUCCESS))\\

/** POP : Dépile un élément.
		Si erreur, tout les bits de la valeur renvoyé sont mis à un,
			et QGM_Error = QGM_NOELEMENTSTACK
		Si pas d'erreur, renvoie tout simplement la donnée **/
#define QGM_STACK_POP(self)\\
((QGM_STACK_EMPTY(self) == 0) ?\\
	((self)->data[--(self)->nElements])\\
:\\
	(QGM_Error = QGM_NOELEMENTSTACK,\\
	 memset((self)->data, 0xFF, sizeof *(self)->data),\\
	 *(self)->data))

/** CLEAR : Remet a 0 la pile **/
#define QGM_STACK_CLEAR(self) (self)->nElements = 0

/** QGM_SIZE : Renvoie le nombre d'élément **/
#define QGM_STACK_SIZE QGM_SIZE

/** EMPTY : Renvoit un si la pile est vide **/
#define QGM_STACK_EMPTY QGM_EMPTY

/** Détruit la pile **/
#define QGM_STACK_QUIT QGM_QUIT

/*****************************************************************************/
/*********************************** QUEUE ***********************************/
/*****************************************************************************/

/** Initialisation :
	Appele QGM_INIT, et met a 0 push et pop
	Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_QUEUE_INIT(self)\\
((QGM_INIT((self)) == QGM_SUCCESS) ?\\
	(((self)->push = (self)->pop = 0, QGM_SUCCESS)) : QGM_BADALLOC)

/** ADJUST : déplace la file au début du bloc de mémoire
	push = nElements;
	pop = 0 **/
#define QGM_QUEUE_ADJUST(self)\\
(memmove((self)->data,\\
		 (self)->data + (self)->pop,\\
		((self)->push - (self)->pop) * sizeof *(self)->data),\\
(self)->push = (self)->nElements,\\
(self)->pop  = 0)

/** PUSH : Ajoute un élément à la file
		Si il n'y a plus de place, on réalloue
		Si on n'arrive à la fin du bloc de mémoire avec la fin de la file
			Alors on ajuste : On déplace la file au début de la mémoire
		On ajoute _data
		Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_QUEUE_PUSH(self, _data)\\
(((self)->nElements == (self)->sizeAlloc) ?\\
	((QGM_RESIZE(self) == QGM_SUCCESS) ?\\
		(((self)->push == (self)->sizeAlloc) ?\\
			(QGM_QUEUE_ADJUST(self),\\
			(self)->data[(self)->push++] = (_data),\\
		  ++(self)->nElements,\\
			 QGM_SUCCESS)\\
		:\\
			((self)->data[(self)->push++] = (_data),\\
		   ++(self)->nElements,\\
			  QGM_SUCCESS))\\
	:\\
		(QGM_Error = QGM_BADALLOC,\\
		 QGM_BADALLOC))\\
:\\
	(((self)->push == (self)->sizeAlloc) ?\\
		(QGM_QUEUE_ADJUST(self),\\
		(self)->data[(self)->push++] = (_data),\\
	  ++(self)->nElements,\\
		 QGM_SUCCESS)\\
	 :\\
	   ((self)->data[(self)->push++] = (_data),\\
	  ++(self)->nElements,\\
		 QGM_SUCCESS)))

/** POP : Défile un élément.
		Si il n'y a plus d'élément, on accèlère la file avec pop = push = 0
			ce qui évite un éventuel déplacement inutile, ou le ralentit

		Si erreur, tout les bits de la valeur renvoyé sont mis à un,
			et QGM_Error = QGM_NOELEMENTQUEUE
		Si pas d'erreur, renvoie tout simplement la donnée **/
#define QGM_QUEUE_POP(self)\\
((QGM_QUEUE_EMPTY(self) == 0) ?\\
	((--(self)->nElements == 0) ?\\
	   ((self)->unusedData = (self)->data[(self)->pop],\\
		(self)->push = (self)->pop = 0,\\
		(self)->unusedData)\\
	:\\
		(self)->data[(self)->pop++])\\
:\\
	(QGM_Error = QGM_NOELEMENTQUEUE,\\
	 memset((self)->data, 0xFF, sizeof *(self)->data),\\
	 *(self)->data))

/** CLEAR : Remet a 0 la file **/
#define QGM_QUEUE_CLEAR(self) (self)->nElements = (self)->push = (self)->pop = 0

/** SIZE : Renvoie le nombre d'élément **/
#define QGM_QUEUE_SIZE QGM_SIZE

/** EMPTY : Renvoit un si la file est vide **/
#define QGM_QUEUE_EMPTY QGM_EMPTY

/** QUIT **/
#define QGM_QUEUE_QUIT QGM_QUIT

/*****************************************************************************/
/*********************************** VECTOR **********************************/
/*****************************************************************************/

/** Initialise un vector **/
#define QGM_VECTOR_INIT QGM_INIT

/** PUSH_BACK : équivalent à un empilement **/
#define QGM_VECTOR_PUSH_BACK QGM_STACK_PUSH

/** PUSH_FRONT : Ajoute un élément au début
		On déplace toutes les données de 1, et on ajoute la donnée
	Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_VECTOR_PUSH_FRONT(self, _data)\\
(((self)->nElements == (self)->sizeAlloc) ?\\
	((QGM_RESIZE(self) == QGM_SUCCESS) ?\\
		(memmove((self)->data + 1,\\
				 (self)->data,\\
				 (self)->nElements++ * sizeof *(self)->data),\\
		*(self)->data = (_data),\\
		QGM_SUCCESS)\\
	:\\
		(QGM_Error = QGM_BADALLOC,\\
		 QGM_BADALLOC))\\
:\\
	(memmove((self)->data + 1,\\
			 (self)->data,\\
			 (self)->nElements++ * sizeof *(self)->data),\\
	 *(self)->data = (_data),\\
	 QGM_SUCCESS))

/** INSERT : Insère un élément
		même principe que PUSH_FRONT, sauf que l'on fait en fonction d'une "place"
	Si pas d'erreur, retourne QGM_SUCCESS, sinon QGM_BADALLOC **/
#define QGM_VECTOR_INSERT(self, _data, place)\\
(((self)->nElements == (self)->sizeAlloc) ?\\
	((QGM_RESIZE(self) == QGM_SUCCESS) ?\\
		(memmove((self)->data + place + 1,\\
				  (self)->data + place,\\
				 ((self)->nElements++ - place) * sizeof *(self)->data),\\
		(self)->data[place] = (_data),\\
		QGM_SUCCESS)\\
	:\\
		(QGM_Error = QGM_BADALLOC,\\
		 QGM_BADALLOC))\\
:\\
	(memmove((self)->data + place + 1,\\
			 (self)->data + place,\\
			((self)->nElements++ - place) * sizeof *(self)->data),\\
	(self)->data[place] = (_data),\\
	QGM_SUCCESS))

/** POP_BACK : équivalent à un dépilement

	Si erreur, tout les bits de la valeur renvoyé sont mis à un,
		et QGM_Error = QGM_NOELEMENTVECTOR
		Si pas d'erreur, renvoie tout simplement la donnée **/
#define QGM_VECTOR_POP_BACK(self)\\
((QGM_VECTOR_EMPTY(self) == 0) ?\\
	((self)->data[--(self)->nElements])\\
:\\
	(QGM_Error = QGM_NOELEMENTVECTOR,\\
	 memset((self)->data, 0xFF, sizeof *(self)->data),\\
	 *(self)->data))

/** POP_FRONT : Retire un élément en début de tableau
		On récupère la donnée, et on déplace le tableau de 1 case vers le début

	Si erreur, tout les bits de la valeur renvoyé sont mis à un,
		et QGM_Error = QGM_NOELEMENTVECTOR
		Si pas d'erreur, renvoie tout simplement la donnée **/
#define QGM_VECTOR_POP_FRONT(self)\\
((QGM_VECTOR_EMPTY(self) == 0) ?\\
	((self)->unusedData = *(self)->data,\\
	  memmove((self)->data,\\
			  (self)->data + 1,\\
			--(self)->nElements * sizeof *(self)->data),\\
	 (self)->unusedData)\\
:\\
	(QGM_Error = QGM_NOELEMENTVECTOR,\\
	 memset((self)->data, 0xFF, sizeof *(self)->data),\\
	 *(self)->data))

/** ERASE : Retire un élément
		même principe que POP_FRONT, sauf que l'on fait en fonction d'une place
	Si erreur, tout les bits de la valeur renvoyé sont mis à un,
		et QGM_Error = QGM_NOELEMENTVECTOR
		Si pas d'erreur, renvoie tout simplement la donnée **/
#define QGM_VECTOR_ERASE(self, place)\\
((QGM_VECTOR_EMPTY(self) == 0) ?\\
	((self)->unusedData = (self)->data[place],\\
	  memmove((self)->data + place,\\
			  (self)->data + place + 1,\\
		   (--(self)->nElements - place)* sizeof *(self)->data),\\
	 (self)->unusedData)\\
:\\
	(QGM_Error = QGM_NOELEMENTVECTOR,\\
	 memset((self)->data, 0xFF, sizeof *(self)->data),\\
	 *(self)->data))

/** CLEAR : Remet a 0 le vector **/
#define QGM_VECTOR_CLEAR(self) (self)->nElements = 0

/** SIZE : Renvoie le nombre d'élément **/
#define QGM_VECTOR_SIZE QGM_SIZE

/** EMPTY : Renvoit un si le vector est vide **/
#define QGM_VECTOR_EMPTY QGM_EMPTY

/** Détruit un vector **/
#define QGM_VECTOR_QUIT QGM_QUIT

#endif
{% endhighlight %}

Qnope nous fournit plusieurs exemples pour tester son code. D'abord un vector.

{% highlight c %}
#include "QGM/alloc.h"
#include "QGM/error.h"
#include "QGM/data.h"

int main(void){
	QGM_VECTOR(int) vectorInt;
	QGM_VECTOR(double) vectorDouble;
	int i;

	if(QGM_VECTOR_INIT(vectorInt) != QGM_SUCCESS)
		return 1;

	if(QGM_VECTOR_INIT(vectorDouble) != QGM_SUCCESS){
		QGM_VECTOR_QUIT(vectorInt);
		return 1;
	}

	for(i = 0; i < 20; ++i)
		if(QGM_VECTOR_PUSH_FRONT(vectorDouble, (double)i / 10.0)
		   != QGM_SUCCESS)
			goto end;

	for(i = 0; i < 20; ++i)
		if(QGM_VECTOR_PUSH_BACK(vectorInt, i) != QGM_SUCCESS)
			goto end;

	/** Affichage du vector double sans rien supprimer **/
	printf("Vector Double\\n");

	for(i = 0; i < 20; ++i)
		printf("%.1f ", vectorDouble->data[i]);
	printf("Il y a %u elements dans le vectorDouble\\n",
		   QGM_VECTOR_SIZE(vectorDouble));

	/** Affichage du vector int en supprimant toutes les données **/
	printf("\\nVector int\\n");

	for(i = 0; i < 20; ++i)
		printf("%d ", QGM_VECTOR_POP_BACK(vectorInt));
	printf("\\nLe vector int est il vide ? %d\\n",
		   QGM_VECTOR_EMPTY(vectorInt));

	end:
	QGM_VECTOR_QUIT(vectorInt);
	QGM_VECTOR_QUIT(vectorDouble);

	return 0;
}
{% endhighlight %}

Puis une pile.

{% highlight c %}
#include "QGM/alloc.h"
#include "QGM/error.h"
#include "QGM/data.h"

int main(void){
	QGM_STACK(double) stack;
	int i;

	if(QGM_STACK_INIT(stack) != QGM_SUCCESS) return 1;

	for(i = 0; i < 20; ++i)
		if(QGM_STACK_PUSH(stack, (double)i / 10.0) != QGM_SUCCESS)
			goto end;

	printf("Il ya %u elements\\n", QGM_STACK_SIZE(stack));
	for(i = 0; i < 21; ++i)
		printf("%.1f ", QGM_STACK_POP(stack));

	printf("\\n%s\\n", QGM_GetError());
	printf("La pile est elle vide?%d", QGM_STACK_EMPTY(stack));

	end:
	QGM_STACK_QUIT(stack);

	return 0;
}
{% endhighlight %}

Et enfin une file.

{% highlight c %}
#include "QGM/alloc.h"
#include "QGM/error.h"
#include "QGM/data.h"

int main(void){
	QGM_QUEUE(double) queue;
	int i;

	if(QGM_QUEUE_INIT(queue) != QGM_SUCCESS) return 1;

	for(i = 0; i < 20; ++i)
		if(QGM_QUEUE_PUSH(queue, (double)i / 10.0) != QGM_SUCCESS)
			goto end;

	printf("il y a %u elements\\n", QGM_QUEUE_SIZE(queue));

	for(i = 0; i < 18; ++i)
		printf("%.1f ", QGM_QUEUE_POP(queue));

	printf("\\nLa queue est elle vide?%d", QGM_QUEUE_EMPTY(queue));

	end:
	QGM_QUEUE_QUIT(queue);

	return 0;
}
{% endhighlight %}
	
## Gestion des exceptions

Je vois déjà ceux qui sautent de leur siège en hurlant à l'hérésie, qu'en C il n'y a pas d'exception. Oui, mais c'était sans compter sur **une bande de fans de C99 et de *longjumps***. Et aussi de détournement de macros. Le résultat n'est évidemment pas aussi souple qu'un langage implémentant naturellement les exceptions tel C++, mais c'est quand même impressionnant et c'est [ici](https://openclassrooms.com/forum/sujet/reproduction-d-un-mecanisme-d-exception-96430) que ça se passe. Je ne vais rien rapatrier parce que y'a vraiment trop de codes.

## Implémenter la libC

Le langage C a une bibliothèque standard assez légère et peu fournie. Il arrive qu'on propose d'en recoder des fonctions en guise d'exercices, le plus connu étant sans doute *printf* et cousins. Mais un membre de l'ex-SdZ s'est dit que ce serait plus marrant de** TOUT recoder**. Le projet n'a jamais abouti, mais il y a quelques jolis codes qui traînent toujours [là-bas](https://openclassrooms.com/forum/sujet/la-libc-des-zeros-58848?page=1). On trouve du printf, de la gestion d'erreurs, des manipulations de chaînes de caractères, du tri, bref, plein de trucs cools.

## Les mots-clefs du C

[Un projet que j'avais initié](https://openclassrooms.com/forum/sujet/les-mots-clefs-du-c-47446), du temps où le SdZ était un site que j'aimais fréquenter. Le but ? Donner une **explication simple et illustrée des différents mot-clefs qui existent en C**. La plupart y sont présents, dont une explication sur *asm* que j'avais pris beaucoup de temps à rédiger et qui me parait assez peu claire avec quelques années de recul (à moins que ce soit parce que c'est l'assembleur qui est un langage peu clair).

## Un ECS en C

Un dernier mais non des moindres, [une implémentation](http://www.gamedev.net/page/resources/_/technical/game-programming/implementing-component-entity-systems-r3382) du principe d'[Entities, Components, Systems](http://entity-systems.wikidot.com/) en C, alors que la plupart des articles sur le sujet que j'ai vu le font en C++. J'aurai l'occasion de revenir sur l'ECS dans d'autres articles.

Voilà, fin de cette série de liens sur ce qu'on peut s'amuser à faire en C. Bon personnellement, je ne le ferai pas parce que je suis habitué au confort apporté par des langages comme C++, Python ou C#. Mais ça prouve que **le C est vraiment un langage à tout faire qu'on peut tordre dans tous les sens pour faire des trucs insensés**.