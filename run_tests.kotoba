#!/usr/bin/env bb
;; tsubasa 翼 — bb-native test runner (Clojure / babashka; no shell). ADR-2606072802.
;;
;; Per the repo-wide rule (root CLAUDE.md §"Operational code = clj/bb"): first-party
;; tooling is clj/bb, NOT shell. This supersedes the former run_tests.sh.
;;
;;   bb test
;;
;; Standalone src and test roots are derived from this file.
(require '[babashka.classpath :as cp]
         '[babashka.fs :as fs]
         '[clojure.test :as t])

(def repo-root (fs/parent (fs/absolutize *file*)))
(cp/add-classpath (str (fs/path repo-root "src")))
(cp/add-classpath (str (fs/path repo-root "test")))

(def suites
  '[tsubasa.methods.test-analyze
    tsubasa.methods.test-kotoba
    tsubasa.methods.test-autorun
    tsubasa.methods.test-seed-integrity
    tsubasa.methods.test-ingest
    tsubasa.methods.test-digest
    tsubasa.methods.test-fetch
    tsubasa.methods.test-identity
    tsubasa.methods.test-kotoba-bridge
    tsubasa.methods.test-openflights
    tsubasa.methods.test-agent
    tsubasa.repository-contract-test])

(apply require suites)

(let [{:keys [fail error]} (apply t/run-tests suites)]
  (if (zero? (+ fail error))
    (println "── tsubasa: ALL suites green ──")
    (do (println "── tsubasa: FAILURES above ──")
        (System/exit 1))))
