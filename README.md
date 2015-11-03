# meajudakkk
BEGIN {
		//Para as verificações prévias foi utilizado expressões regulares
		FS ="[\(\)[:space:](?:==)\+\>\<(?:!=)]"
		errors=0;
}
{
	if($0 ~/^(int|char).*\=.*;/) {
		//após verificação utilizou-se funções do próprio awk
		//a função match() retorna o indice de onde foi encontrado o que se quer procurar
		begin =  match($0, /\=/)  
		value  = substr($0, begin+1)
		print "Declarou " $2 " como " value
	}else if($0 ~/^if/) {
		if($0 ~/^if[[:space:]]*\(.*[(?:==)\+\>\<].*\)/ || $0 ~/^if[[:space:]]*\([a-zA-Z0-9]+\)/){
			begin =  match($0, /\(/) 
			end =  match($0, /\)/) 
			print substr($0, begin+1 , end-begin-1)
		}else{
			print "Erro na linha " NR-1 ", " $0
			errors++;
		}
	}else{	
		//NR-1 variável padrão do awk para verificar a linha
		print "Erro na linha " NR-1 ", " $0
		errors++;
	}
}
END {
	if(errors>0){
		print errors " errors in your code";
	}else{
		print "Success";
	}
}
