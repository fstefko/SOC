# Build stage
FROM dart:stable AS build

WORKDIR /app

# Dependencies
COPY pubspec.* ./
RUN dart pub get

# Install Dart Frog CLI
RUN dart pub global activate dart_frog_cli
ENV PATH="/root/.pub-cache/bin:${PATH}"

# Copy source and generate production server
COPY . .
RUN dart_frog build

# Compile server
RUN dart pub get --offline
RUN dart compile exe build/bin/server.dart -o build/bin/server

# Runtime stage
FROM scratch

COPY --from=build /runtime/ /
COPY --from=build /app/build/bin/server /app/bin/server

EXPOSE 8080

CMD ["/app/bin/server"]