FROM debian:jessie

RUN apt-get update \
	&& apt-get install -y --no-install-recommends ca-certificates git \
	&& rm -rf /var/lib/apt/lists/*

# http://julialang.org/juliareleases.asc
# Julia (Binary signing key) <buildbot@julialang.org>
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 3673DF529D9049477F76B37566E3C7DC03D6E495

ENV JULIA_PATH /usr/local/julia
ENV JULIA_VERSION 0.4.3

RUN mkdir $JULIA_PATH \
	&& apt-get update && apt-get install -y curl \
	&& curl -sSL "https://julialang.s3.amazonaws.com/bin/linux/x64/${JULIA_VERSION%[.-]*}/julia-${JULIA_VERSION}-linux-x86_64.tar.gz" -o julia.tar.gz \
	&& curl -sSL "https://julialang.s3.amazonaws.com/bin/linux/x64/${JULIA_VERSION%[.-]*}/julia-${JULIA_VERSION}-linux-x86_64.tar.gz.asc" -o julia.tar.gz.asc \
	&& gpg --verify julia.tar.gz.asc \
	&& tar -xzf julia.tar.gz -C $JULIA_PATH --strip-components 1 \
	&& rm -rf /var/lib/apt/lists/* julia.tar.gz*

ENV PATH $JULIA_PATH/bin:$PATH

RUN apt-get update && apt-get install -y bzip2 build-essential \
	&& julia -e 'Pkg.add("Nemo"); using Nemo' \
	&& apt-get remove -y bzip2 build-essential \
	&& echo "using Nemo" > /root/.juliarc.jl

CMD ["julia"]
