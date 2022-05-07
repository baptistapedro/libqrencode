FROM fuzzers/afl:2.52

RUN apt-get update
RUN apt install -y build-essential wget git clang cmake \
    autoconf automake autotools-dev libtool pkg-config libpng-dev
RUN git clone https://github.com/fukuchi/libqrencode
WORKDIR /libqrencode
RUN cmake -DCMAKE_C_COMPILER=afl-clang -DCMAKE_CXX_COMPILER=afl-clang++ -DBUILD_SHARED_LIBS=1 .
RUN make
RUN make install
RUN cp ./qrencode /qrencode
RUN mkdir /qr-corpus
RUN wget https://file-examples.com/wp-content/uploads/2017/10/file_example_PNG_500kB.png
RUN wget https://file-examples.com/wp-content/uploads/2017/10/file_example_PNG_1MB.png
RUN wget https://people.sc.fsu.edu/~jburkardt/data/png/ajou_logo.png
RUN wget https://people.sc.fsu.edu/~jburkardt/data/png/all_gray.png
RUN wget https://people.sc.fsu.edu/~jburkardt/data/png/brain.png
RUN wget https://people.sc.fsu.edu/~jburkardt/data/png/coins.png
RUN wget https://people.sc.fsu.edu/~jburkardt/data/png/dla.png
RUN cp *.png /qr-corpus

ENTRYPOINT ["afl-fuzz", "-i", "/qr-corpus", "-o", "/out"]
CMD ["/qrencode", "-r", "@@", "-o", "/dev/null"]
