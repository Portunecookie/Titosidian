1. 한국어 모델 선정
	* EEVE, Solar 중에 채택
	* Solar은 llamacpp가 안된다는 이슈가 있는데 트라이해보자.
2. 모델 gguf로 쉽게 변환해주는 사이트를 찾음
	* https://huggingface.co/spaces/ggml-org/gguf-my-repo
	* Solar STOCK, EEVE 둘다 같이 변환
		* https://huggingface.co/yanolja/EEVE-Korean-10.8B-v1.0
		* https://huggingface.co/yanolja/EEVE-Korean-Instruct-10.8B-v1.0
	* 안된다
	* ![[Pasted image 20240702232219.png]]
		* 뉴저지 라고 대답하는 경우도 있었음
	* EEVE로 트라이해보니까 된다
	* ![[Pasted image 20240702232514.png]]
3. 결론 : EEVE를 gguf로 직접 변환해서 사용
	* 양자화 정도를 많이 높여서 라즈베리파이 오면 사용 예정

4. Modelfile 문법 공부
	* 공식 사이트에서 공부하였음
	* 생각보다 조작할 수 있는 수치가 다양하였으나, template이 유의미하지는 않을 것으로 판단.
	* 추후 python 통해서 ollama 호출, model을 이름을 통해서만 호출하고 뒤에 있는 prompt는 langchain 잘 사용해서 해결 예정.
