FROM nvidia/cuda:11.1.1-cudnn8-devel
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH
ENV DEBIAN_FRONTEND noninteractive
RUN apt-key adv --fetch-keys "https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub"
RUN apt-get update && \
	apt-get install -y git cmake uuid-runtime lsb-release wget software-properties-common && \
	wget --quiet https://golang.org/dl/go1.16.6.linux-amd64.tar.gz -O ~/go.tar.gz && \
	tar xzf ~/go.tar.gz -C /opt/ && \
	ln -s /opt/go/bin/go /usr/local/bin/go && \
	rm ~/go.tar.gz
RUN wget --quiet https://apt.llvm.org/llvm.sh -O ~/llvm.sh && \
	chmod +x ~/llvm.sh && \
	~/llvm.sh 11 && \
	rm ~/llvm.sh
RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh -O ~/anaconda.sh && \
	/bin/bash ~/anaconda.sh -b -p /opt/conda && \
	rm ~/anaconda.sh && \
	ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc && \
    /opt/conda/bin/conda clean -afy
RUN conda install -y pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
RUN git clone https://github.com/NVIDIA/apex.git ~/apex && \
	cd ~/apex && \
	bash -c "source /opt/conda/etc/profile.d/conda.sh && conda activate && python setup.py install && pip install pyelftools" && \
	rm -rf ~/apex
RUN wget --quiet https://github.com/avast/retdec/releases/download/v4.0/retdec-v4.0-ubuntu-64b.tar.xz -O ~/retdec.tar.xz && \
	tar xf ~/retdec.tar.xz -C /opt/ && \
	ln -s /opt/retdec/bin/retdec-decompiler.py /usr/local/bin/retdec-decompiler && \
	rm ~/retdec.tar.xz
CMD [ "/bin/bash" ]
