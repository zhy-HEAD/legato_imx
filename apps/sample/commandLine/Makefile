TARGETS := $(MAKECMDGOALS)

.PHONY: all $(TARGETS)
all: $(TARGETS)

$(TARGETS):
	mkapp -v -t $@ \
		fileInfo.adef

clean:
	rm -rf _build_* *.update

